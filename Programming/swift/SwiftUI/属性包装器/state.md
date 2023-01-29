# @state

[官方文档](https://developer.apple.com/documentation/swiftui/state)

## 概述

SwiftUI将会管理作为@state属性的值，当值发生变化时，SwiftUI会更新View结构中依赖值的部分

## 注意

1. 使用@State的变量传递给子视图时，子视图无法修改其变量（如有需要请用[@Binding](https://developer.apple.com/documentation/swiftui/binding)）
2. 不要在视图层次结构中实例化视图的位置初始化视图的状态属性，因为这可能与 SwiftUI 提供的存储管理冲突。 为避免这种情况，始终将状态声明为私有，并将其放置在需要访问该值的视图层次结构中的最高视图中。

## 用法

```swift
struct PlayButton: View {
    @State private var isPlaying: Bool = false

    var body: some View {
        Button(isPlaying ? "Pause" : "Play") {
            isPlaying.toggle()
        }
    }
}
```

