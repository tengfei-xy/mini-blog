# tcp监听http

net.ListenAndServeTLS（3128）确定监听地址、证书文件、建立结构体

```go
func ListenAndServeTLS(addr, certFile, keyFile string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServeTLS(certFile, keyFile)
}
```

### 

ListenAndServeTLS（3148）开始监听

```go
/*
ListenAndServeTLS监听TCP网络地址srv。
然后调用ServeTLS处理传入的TLS连接上的请求。
已接受的连接配置为启用TCP保持活动。
如果不提供服务器的TLSConfig，则必须提供包含证书和匹配的服务器私钥的文件名。
证书和TLSConfig。GetCertificate填充。
如果证书是由证书颁发机构签署的，那么certFile应该是服务器证书、任何中间体和CA证书的连接。
如果srv.Addr地址为空，使用“:https”。ListenAndServeTLS总是返回一个non-nil错误。
关闭或关闭后，返回的错误是ErrServerClosed。
*/
func (srv *Server) ListenAndServeTLS(certFile, keyFile string) error {
	if srv.shuttingDown() {
		return ErrServerClosed
	}
	addr := srv.Addr
	if addr == "" {
		addr = ":https"
	}

	ln, err := net.Listen("tcp", addr)
	if err != nil {
		return err
	}

	defer ln.Close()
	// 如果非TLS
    // return srv.Serve(ln)
	return srv.ServeTLS(ln, certFile, keyFile)
}
```



ServeTLS(2698） 安装TLS监听器

```go
/*
ServeTLS在侦听器l上接受进入的连接，为每个连接创建一个新的服务goroutine。
服务goroutines执行TLS设置，然后读取请求，调用srv。处理程序来响应它们。
如果不是服务器的TLSConfig，则必须提供包含证书和与服务器匹配的私钥的文件。
证书和TLSConfig。GetCertificate填充。
如果证书是由证书颁发机构签署的，那么certFile应该是服务器证书、任何中间体和CA证书的连接。
ServeTLS总是返回一个非nil错误。关闭或关闭后，返回的错误是ErrServerClosed。
*/
func (srv *Server) ServeTLS(l net.Listener, certFile, keyFile string) error {
	// 在srv之前安装HTTP/2。用于初始化srv。在克隆它并创建TLS侦听器之前。
	if err := srv.setupHTTP2_ServeTLS(); err != nil {
		return err
	}

	config := cloneTLSConfig(srv.TLSConfig)
	if !strSliceContains(config.NextProtos, "http/1.1") {
		config.NextProtos = append(config.NextProtos, "http/1.1")
	}

	configHasCert := len(config.Certificates) > 0 || config.GetCertificate != nil
	if !configHasCert || certFile != "" || keyFile != "" {
		var err error
		config.Certificates = make([]tls.Certificate, 1)
		config.Certificates[0], err = tls.LoadX509KeyPair(certFile, keyFile)
		if err != nil {
			return err
		}
	}

	tlsListener := tls.NewListener(l, config)
	return srv.Serve(tlsListener)
}
```



Serve（2907）

```go
/*
Serve在侦听器l上接受进入的连接，为每个连接创建一个新的服务goroutine。
服务goroutines读取请求然后调用srv。处理程序来响应它们。
HTTP/2支持只在侦听器返回*tls时启用。
在TLS Config.NextProtos中使用“h2”配置Conn连接。
服务总是返回一个非nil错误并关闭l。
关闭或关闭后，返回的错误是ErrServerClosed。
*/
func (srv *Server) Serve(l net.Listener) error {
	if fn := testHookServerServe; fn != nil {
		fn(srv, l) // call hook with unwrapped listener
	}

	origListener := l
	l = &onceCloseListener{Listener: l}
	defer l.Close()

	if err := srv.setupHTTP2_Serve(); err != nil {
		return err
	}

	if !srv.trackListener(&l, true) {
		return ErrServerClosed
	}
	defer srv.trackListener(&l, false)

	baseCtx := context.Background()
	if srv.BaseContext != nil {
		baseCtx = srv.BaseContext(origListener)
		if baseCtx == nil {
			panic("BaseContext returned a nil context")
		}
	}

	var tempDelay time.Duration // how long to sleep on accept failure

	ctx := context.WithValue(baseCtx, ServerContextKey, srv)
	for {
		rw, err := l.Accept()
		if err != nil {
			select {
			case <-srv.getDoneChan():
				return ErrServerClosed
			default:
			}
			if ne, ok := err.(net.Error); ok && ne.Temporary() {
				if tempDelay == 0 {
					tempDelay = 5 * time.Millisecond
				} else {
					tempDelay *= 2
				}
				if max := 1 * time.Second; tempDelay > max {
					tempDelay = max
				}
				srv.logf("http: Accept error: %v; retrying in %v", err, tempDelay)
				time.Sleep(tempDelay)
				continue
			}
			return err
		}
		connCtx := ctx
		if cc := srv.ConnContext; cc != nil {
			connCtx = cc(connCtx, rw)
			if connCtx == nil {
				panic("ConnContext returned nil")
			}
		}
		tempDelay = 0
		c := srv.newConn(rw)
		c.setState(c.rwc, StateNew) // before Serve can return
		go c.serve(connCtx)
	}
}
```

