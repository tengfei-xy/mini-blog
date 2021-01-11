```go

// 关于 操作时间 的 结构体
type DoTime struct{
	Str								string
	T 								time.Time
}
// 判断 是否早于i小时
// 返回 是：早于i小时
// 返回 否：晚于i小时
func (dt * DoTime) base(){
	if dt.Str != ""{
		time.Parse("2020-05-24 17:06:47 +0800 CST",dt.Str)
	}
}
func (dt * DoTime) BeforeHour(i int) bool{
	var err error
	if dt.Str != ""{
		dt.T,err = time.Parse("2006-01-02 15:04:05.999999999 -0700 MST",dt.Str)
		if err !=nil {
			PntError(err)
			return false
		}
		if dt.T.Add(time.Duration(5)*time.Hour).Before(time.Now()){
			return true
		}
	}
	return false
}
```

