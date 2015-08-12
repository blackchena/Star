# Star
Some Difficulties encountered in development
## 银联支付控件调用crash问题
简单说来，加了这个参数后，链接器就会把静态库中所有的Objective-C类和分类都加载到最后的可执行文件中，虽然这样可能会因为加载了很多不必要的文件而导致可执行文件变大，但是这个参数很好地解决了我们所遇到的问题。但是事实真的是这样的吗？

如果-ObjC参数真的这么有效，那么事情就会简单多了。

Important: For 64-bit and iPhone OS applications, there is a linker bug that prevents -ObjC from loading objects files from static libraries that contain only categories and no classes. The workaround is to use the -allload or -forceload flags.

当静态库中只有分类而没有类的时候，-ObjC参数就会失效了。这时候，就需要使用-all_load或者-force_load了。

-all_load会让链接器把所有找到的目标文件都加载到可执行文件中，但是千万不要随便使用这个参数！假如你使用了不止一个静态库文件，然后又使用了这个参数，那么你很有可能会遇到ld: duplicate symbol错误，因为不同的库文件里面可能会有相同的目标文件，所以建议在遇到-ObjC失效的情况下使用-force_load参数。

-force_load所做的事情跟-all_load其实是一样的，但是-force_load需要指定要进行全部加载的库文件的路径，这样的话，你就只是完全加载了一个库文件，不影响其余库文件的按需加载。
## 静态库访问资源文件
```objective-c
NSString *path = [[NSBundle mainBundle] pathForResource:@"Myframework" ofType:@"bundle"];
NSBundle *resourcesBundle = [NSBundle bundleWithPath:path];
As for images you can just prepend Myframework.bundle/ to their names:
[UIImage imageNamed:@"Myframework.bundle/MyImage"
```
##NSDate 封装
https://github.com/MatthewYork/DateTools

##下拉刷新
CBStoreHouseRefreshControl

