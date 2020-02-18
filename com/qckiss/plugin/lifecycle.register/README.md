# Android组件化之组件/模块初始化注册到application；组件生命周期注册

# LiblifecycleRegisterPlugin

```yaml
    一个用于帮助 Android App 进行组件化改造插件工具 ——  使业务组件,功能组件library 可以感知到主工程Application的创建与退出；
    在Android组件化开发中，必会经历的问题之一：组件/模块初始化问题。组件化为了解耦，每个组件/模块有不同的功能，例如不同组件/模块要在application中初始化一些第三方SDK或本组件/模块需要在application初始化时做一些操作。
```

[English](https://github.com/qcks/public-maven-repo/blob/master/com/qckiss/plugin/lifecycle.register/README.md)

---

[TOC]



#### 最新版本

| 模块     | com.qckiss.library:lifecycle.proxy | com.qckiss.plugin:lifecycle.register |
| -------- | ---------------------------------- | ------------------------------------ |
| 最新版本 | 1.0.0                              | 1.0.0                                |

#### 一、功能介绍
**library 可以感知到主工程Application的创建**，从而做自身需要的操作

已开启Transform增量编译

#### 二、基础功能
1. 添加依赖和配置
   
    ```groovy
    //主工程中
        apply plugin: 'com.android.application'
        apply plugin: 'com.qckiss.plugin.lifecycleregister
        //扫描配置
        scanPackageNames {
            scanAll = false //默认false;扫描 names中包含的包名library,true扫描所有
            names = ["com/xxx1","com/xxx2"] //包名分割符“/”
        }
    ```
    
    ``` groovy
    //主工程和各library
    dependencies {
            //替换为最新版本号
            implementation 'com.qckiss.library:lifecycle.proxy:x.x.x'
        }
    ```
    
    ```groovy
        //项目build.gradle
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
    
    
    
2. Library中添加标记注解@MarkLibApplictionProxy

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
            //暂未实现
            Log.i("ModelA", "ApplicationProxy onExitAllActivity");
        }
    }
    ```

3. 壳子Application的onCreate方法添加标记注解@MarkRecaptureLibProxy

    ```java
    
    public class MyApplication extends BaseApplication {
        @MarkRecaptureLibProxy
        @Override
        public void onCreate() {
            super.onCreate();
        }
    }
    ```

4. 添加混淆规则(如果使用了Proguard)

    ``` 
    -keep public class  com.qckiss.liblifecycleproxy.**{*;}
    -keep class * implements com.qckiss.liblifecycleproxy.IApplicationProxy
    ```

#### 三、操作步骤

```

点赞加星小礼物
复制粘贴人人爱
如有 BUG记小本
携尔长刀来相砍

```



