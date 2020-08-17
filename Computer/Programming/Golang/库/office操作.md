[godoc](https://godoc.org/github.com/unidoc/unioffice/document#Run)

## 例:新建保存

```go
doc := document.New()
para := doc.AddParagraph()
run := para.AddRun()
run.SetText("foo")
doc.SaveToFile("foo.docx")
```

