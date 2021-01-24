# SwiftuiResource
SwiftUI 学习资料整理

关于属性装饰器@State @Bingding @ObservedObject @EnvironmentObject [cnblogs](https://www.cnblogs.com/xiaoniuzai/p/11417123.html)

##View之间通讯模式## [英文](https://www.vadimbulavin.com/passing-data-between-swiftui-views/)
1.父视图 ==> 子视图 单向，使用初始化方法。
2.祖先视图 ==> 子孙视图 双向，使用Environment，本质是应用范围内的字典对象。

让我们看看如何将图像缓存注入环境。它的实现如下：
```swift
protocol ImageCache {
    subscript(_ key: String) -> UIImage? { get set }
}

struct TemporaryImageCache: ImageCache {
    private let cache = NSCache<NSString, UIImage>()
    
    subscript(_ key: String) -> UIImage? {
        get { cache.object(forKey: key as NSString) }
        set { newValue == nil ? cache.removeObject(forKey: key as NSString) : cache.setObject(newValue!, forKey: key as NSString) }
    }
}
```
现在将图像缓存添加到环境中：
```swift
struct ImageCacheKey: EnvironmentKey {
    static let defaultValue: ImageCache = TemporaryImageCache()
}

extension EnvironmentValues {
    var imageCache: ImageCache {
        get { self[ImageCacheKey.self] }
        set { self[ImageCacheKey.self] = newValue }
    }
}
```
使用@Environment 属性包装器从环境中读取一个值：
```swift
struct TodoItemDetail: View {
    let item: TodoItem
    @Environment(\.imageCache) var cache: ImageCache
    
    var body: some View {
        ...
    }
}
```
3.子视图 ==> 父视图,使用绑定和回调
  单向：使用回调
  双向：使用绑定
  **回调** 
  ```
  struct TodoListView: View {
    let items: [TodoItem]
    
    var body: some View {
        List(items) { item in
            TodoItemView(item: item) {
                print("Detail selected", item)
            }
        }
    }
}

struct TodoItemView: View {
    let onDetail: () -> Void
    ...

    var body: some View {
        ...
        Button(action: onDetail) { ... }
    }
}
```


