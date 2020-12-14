# 表格

## 含表头

### wxml

```
  <view class="table">
    <view class="tr bg-w">
    <view class="th">head1</view>
  </view>
  <block>
    <view class="tr bg-g">
      <view class="td"></view>
    </view>
    <view class="tr">
      <view class="td"></view>、
    </view>
  </block>
  </view>
```

### wcss

```
.table {
 border: 0px solid darkgray;
}
.tr {
 display: flex;
 width: 100%;
 justify-content: center;
 height: 3rem;
 align-items: center;
}
.td {
 width:40%;
 justify-content: center;
 text-align: center;
}
.bg-w{
 background: snow;
}
.bg-g{
 background: #E6F3F9;
}
.th {
 width: 40%;
 justify-content: center;
 background: #3366FF;
 color: #fff;
 display: flex;
 height: 3rem;
 align-items: center;
}
```

## 纯粹间隔

### wcss

```
.table {
 }
 .tr {
  display: flex;
  width: 100%;
  justify-content: center;
  height: 3rem;
  align-items: center;
 }
 .td {
  width:40%;
  justify-content: center;
  text-align: center;
 }
 .bg-w{
 }
 .bg-g{
 }
 .th {
  width: 40%;
  justify-content: center;
  display: flex;
  height: 3rem;
  align-items: center;
 }
```

