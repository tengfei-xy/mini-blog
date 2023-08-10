# for

打印附加参数

```powershell
For($i=0;$i -lt $args.Count; $i++)
{
    Write-Host "parameter $i : $($args[$i])"
}
```

遍历

```纯文本
foreach ($i in $colItems){

}
```
