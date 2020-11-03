设置代理ip

git config --global https.proxy "127.0.0.1:52803"

git config --global http.proxy 'socks5://127.0.0.1:52804'



查看代理

git config --global --get http.proxy



取消代理:

git config --golbal --unset http.proxy