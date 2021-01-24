# SwiftuiResource
SwiftUI 学习资料整理

关于属性装饰器@State @Bingding @ObservedObject @EnvironmentObject [cnblogs](https://www.cnblogs.com/xiaoniuzai/p/11417123.html)

##View之间通讯模式## [英文](https://www.vadimbulavin.com/passing-data-between-swiftui-views/)
父视图 ==> 子视图 单向，使用初始化方法。
祖先视图 ==> 子孙视图 双向，使用Environment，本质是应用范围内的字典对象。

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

