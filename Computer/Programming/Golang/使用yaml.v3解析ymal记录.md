# 使用yaml.v3 解析ymal记录
## 准备环境
`go get gopkg.in/yaml.v3`
## config.yaml
basic:
 sendemail: "taotengfei@zhiweidata.com"
mysql:
 ip: "127.0.0.1"
 enbaledip: flase
 unixsocket: "/tmp/mysql.sock"
 enableunixsocket: true
inst:
- 
 title: "crgk"
 explain : "成人高考"
 url: "https://www.zjzs.net/moban/index/2c9081f061d15b160161d165e6b10014_tree.html"
 method: "Get"
- 
 title: "qidiansx"
 explain : "圣墟"
 url: "https://book.qidian.com/info/1004608738"
 method: "Get"
## yaml struct
type info struct{
	
	Basic 				 		`yaml:"basic"`
	Mysql 						`yaml:"mysql"`
	Inst			[]Section 	`yaml:"inst"`
}
type Basic struct{
	Sendemail			string 	`yaml:"sendemail"`
}
type Mysql struct{
	IP					string 	`yaml:"ip"`
	EnableIP			bool 	`yaml:"enableip"`	
	UnixSocket			string 	`yaml:"unixsocket"`
	EnableUnixSocket	bool	`yaml:"enableunixsocket"`
}

type Section struct{
	Title				string 	`yaml:"title"`
	Explain				string	`yaml:"explain:"`
	Url					string	`yaml:"url"`
	Method				string 	`yaml:"method"`
}

## 代码解析
yamlFile, err := ioutil.ReadFile(*conf)
if err != nil {
	return fmt.Errorf("yamlFile.Get err   #%v " ,err)
}
err = yaml.Unmarshal(yamlFile, &configInfo)
if err != nil {
	return fmt.Errorf("Unmarshal: %v", err)
}
log.Println(configInfo)