#### 学习Kotlin,JetPack,协程的Wanandroid客户端
- **Kotlin语言编程，JetPack组件架构，Material Design风格**
- **repository层实现业务逻辑，viewmodel控制视图逻辑，并用koin框架依赖注入**
- **使用Retrofit，ROOM（用户信息，阅读历史）,协程构建数据层**  
#### 亮点功能
- 参考bilili启动页style适配
- 仿酷安动态换肤，夜间模式(灵活使用theme及系统属性,用最少的资源文件和代码实现动态换肤，可参考https://juejin.im/post/6844904200673968141)
- 用户信息，阅读历史使用room数据库，配合flow,livedata,实现可观察性
#### 效果图展示 
![项目效果图](https://source.acexy.cn/view/XQXWOxb)
 #### APK下载：
- [Github下载https://github.com/wwy863399246/WanAndroid/releases/download/1.0.0/app-debug.apk)
## 1.添加依赖后如何使用koin依赖注入viewmodel,repository
- **1.1 新建AppModulel类**
```
val viewModelModule = module {
    viewModel { HomePageViewModel(get(), get()) }
}
val repositoryModule = module {
    single { RemoteDataSource() }
    single { ArticleUserCase(get()) }
    single { MainRepository(get()) }
}
val appModule = listOf(viewModelModule, repositoryModule)
```
- **1.2 MyApplication**
```
startKoin {
            androidLogger(Level.INFO)
            androidContext(this@MyApplication)
            modules(appModule)
        }
```
- **1.2 Repository，Viewmodel**
```
class ArticleUserCase(private val remoteDataSource: RemoteDataSource) {
}
class MainRepository(private val homeRemoteDataSource: RemoteDataSource) {
}
class HomePageViewModel(private val mainRepository : MainRepository, private val articleUserCase:ArticleUserCase) : BaseViewModel() {
}
```
- **1.2 Activity,Fragment中**
```
class HomePageFragment : BaseVMFragment<HomePageViewModel>(), OnLoadMoreListener {
   override fun initVM(): HomePageViewModel = getViewModel()
}
```