```go
func parseHtmlDocs(umsg * []byte) []byte{
	strDocs:=string(*umsg)
	docs :=make(map[string]string)
	docSeq :=make(map[int]string)
	
	
	for j,i := range strings.Split(strDocs,"\r\n"){
		//PrintLog(i)
		if j == 0{
			headline := strings.Split(i," ")
			docs["act"] = headline[0]
			docSeq[j] = "act"
			docs["link"] = headline[1][2:]
			docSeq[1] = "link"
			docs["version"] = headline[2]
			docSeq[2] = "version"
			continue
		}else{
			var last = j+3
			if strings.Index(i,": ") != -1 {
				head := strings.Split(i,": ")
				docs[head[0]] = head[1]
				docSeq[last] = head[0]
				continue
			}else{
				docs["data"] = i
				docSeq[last+1] = "data"

				continue
			}
		}
	}
	// for i:=0;i< len(docSeq);i++{
	// 	PrintLog(docs[docSeq[i]])
	// }
	
	// 建立 企业微信 加密方法
	wxcpt := wxbizmsgcrypt.NewWXBizMsgCrypt(WXTokenMsgSend, WxworkEncodingAseKey, WxworkReceiverId, wxbizmsgcrypt.XmlType)

	// 验证消息
	list := strings.Split(docs["link"],"&")
	msg_signature := strings.Split(list[0],"=")[1]
	timestamp := strings.Split(list[1],"=")[1]
	nonce := strings.Split(list[2],"=")[1]
	echostr := strings.Split(list[3],"=")[1]
	parseStr,_ :=url.QueryUnescape(echostr)
	
	res, cryptErr := wxcpt.VerifyURL(msg_signature,timestamp, nonce,parseStr)
	if nil != cryptErr {
        PrintLog("verifyUrl fail", cryptErr)
    }
	//PrintLog("verifyUrl success code:", string(res))

	//PrintLog("\n\n\n\n\n")

	//var res []byte
	// for 
	time := time.Now().Add(-8 * time.Hour).Format(time.RFC1123)
	time = strings.Replace(time,"CST","GMT",1)
	result := []byte("HTTP/1.1 200 OK\r\nDate: "+time+"\r\nContent-Length: 19\r\nContent-Type: text/plain; charset=utf-8\r\n\r\n" + string(res))
	//result := []byte("HTTP/1.1 200 OK\r\nContent-Type:text/html\r\nContent-Length:19\r\n\r\n" + string(res))
	
	return result
}
```

