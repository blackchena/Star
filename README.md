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

##不要在init和dealloc函数中使用accessor(Don’t Use Accessor Methods in Initializer Methods and dealloc)
init 里面不用 setter 不是内存问题，而是 setter 可能会触发其他的逻辑，例如重写的 setter 方法或者 KVC，将可能调用其他还没来得及 init 的变量，最终导致不可预计行为！
- [参考文献翻译](http://joywii.github.io/blog/2015/03/05/bu-yao-zai-objective-cde-inithe-dealloczhong-xiang-zi-ji-fa-song-xiao-xi/) 
- [唐巧](http://blog.devtang.com/blog/2011/08/10/do-not-use-accessor-in-init-and-dealloc-method/)
- [上面引用](http://dijkst.github.io/blog/2013/12/07/bu-yao-zai-initializer-he-dealloc-fang-fa-zhong-shi-yong-setter-he-getter/)
- http://qualitycoding.org/objective-c-init/

##Xcode 设置
###环境变量
- https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html#//apple_ref/doc/uid/TP40003931-CH3-SW105
##CoreData
临时的NSManagedObject
- [如何用Temporary Deal NSManagedObject实例](http://www.helplib.com/qa/587047)
- [如何应对临时NSManagedObject实例](http://www.ophome.cn/question/17483)

##宏定义
#define语句中的#是把参数字符串化，##是连接两个参数成为一个整体。
#define FACTORY_REF(name) { #name, Make##name }
中#name就是将传入的name进行字符串化，Make##name就是将Make跟name进行连接，使它们成为一个整体。
```objective-c
#define FACTORY_CREATE(name) \
static sp<MediaSource> Make##name(const sp<MediaSource> &source) { \ 
return new name(source); \
}
#define FACTORY_CREATE_ENCODER(name) \
static sp<MediaSource> Make##name(const sp<MediaSource> &source, const sp<MetaData> &meta) { \ 
return new name(source, meta); \
}
#define FACTORY_REF(name) 
{#name,Make##name},
FACTORY_CREATE(MP3Decoder)
FACTORY_CREATE(AMRNBDecoder)
FACTORY_CREATE(AMRWBDecoder)
FACTORY_CREATE(AACDecoder)
FACTORY_CREATE(AVCDecoder)
FACTORY_CREATE(G711Decoder)
FACTORY_CREATE(M4vH263Decoder)
FACTORY_CREATE(VorbisDecoder)
FACTORY_CREATE(VPXDecoder)
FACTORY_CREATE_ENCODER(AMRNBEncoder)
FACTORY_CREATE_ENCODER(AMRWBEncoder)
FACTORY_CREATE_ENCODER(AACEncoder)
FACTORY_CREATE_ENCODER(AVCEncoder)
FACTORY_CREATE_ENCODER(M4vH263Encoder)
```
##应用被拒
 MFi - Program Authorization 
external-accessory 在你的info.list 
##Conflicting constraints debug
Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger
