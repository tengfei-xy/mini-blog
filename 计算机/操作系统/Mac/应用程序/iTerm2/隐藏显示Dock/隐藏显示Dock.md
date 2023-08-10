# 隐藏显示Dock

隐藏Dock

```纯文本
/usr/libexec/PlistBuddy  -c "Add :LSUIElement bool true" /Applications/iTerm.app/Contents/Info.plist
```

显示Dock

```纯文本
/usr/libexec/PlistBuddy  -c "Delete :LSUIElement" /Applications/iTerm.app/Contents/Info.plist
```
