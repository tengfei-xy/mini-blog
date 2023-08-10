# CSS

## 目录

-   [CSS操作](#CSS操作)
    -   [设置CSS样式](#设置CSS样式)
        -   [单个样式](#单个样式)
        -   [多个样式](#多个样式)
    -   [返回CSS样式](#返回CSS样式)

# CSS操作

## 设置CSS样式

### 单个样式

`$("div").css("color","yellow");`

### 多个样式

`$("div").css({"background-color":"yellow","font-size":"200%"});`

```css
var css = {
    background-color: '#EEE',
    height: '500px',
    margin: '10px',
    padding: '2px 5px'
};
$("div").css(css)
```

## 返回CSS样式

`console.log($("div").css("color"));`
