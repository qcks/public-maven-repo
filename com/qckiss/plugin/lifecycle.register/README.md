# LiblifecycleRegisterPlugin
```
    一个用于帮助 Android App 进行组件化改造插件工具 ——  使业务组件,功能组件library 可以感知到主工程Application的创建与退出
```

[English]()

---

#### 最新版本

| 模块     | com.qckiss.library:lifecycle.proxy | com.qckiss.plugin:lifecycle.register |
| -------- | ---------------------------------- | ------------------------------------ |
| 最新版本 | 1.0.0                              | 1.0.0                                |

#### 一、功能介绍
1. **library 可以感知到主工程Application的创建**

#### 二、基础功能
1. 添加依赖和配置
    ``` gradle
    主工程中
        apply plugin: 'com.android.application'
        apply plugin: 'com.qckiss.plugin.lifecycleregister
        //扫描配置
        scanPackageNames {
            scanAll = false //默认false扫描 names中包含的包名library,true扫描所有
            names = ["com/xxx1","com/xxx2"] //包名分割符“/”
        }
    
    主工程和各library
    dependencies {
            //替换为最新版本号
            implementation 'com.qckiss.library:lifecycle.proxy:x.x.x'
        }
        
    项目build.gradle
    
        dependencies {
        	...
            classpath 'com.qckiss.plugin:lifecycle.register:x.x.x'
        }
	
        allprojects {
            repositories {
                maven{
                    url 'https://raw.githubusercontent.com/qcks/public-maven-repo/master'
                }
            }
        }
    ```
    
2. 添加标记注解
    ``` java
    /**
     * priority 值越小 注册优先级越高
     */
    @MarkLibApplictionProxy(priority = 2)
    public class ModelaApplicationProxy implements IApplicationProxy {
        @Override
        public void onCreate(Application application) {
        Log.i("ModelA", "ApplicationProxy onCreate");
        }
    
        @Override
        public void onExitAllActivity() {
            Log.i("ModelA", "ApplicationProxy onExitAllActivity");
        }
    }
    ```

5. 添加混淆规则(如果使用了Proguard)
    ``` 
    -keep public class  com.qckiss.liblifecycleproxy.**{*;}
    -keep class * implements com.qckiss.liblifecycleproxy.IApplicationProxy
    ```
