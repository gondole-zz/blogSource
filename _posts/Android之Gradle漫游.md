title: Android之Gradle漫游  
date: 2016-04-19 16:39:41  
tags: [Android, Gradle, 多渠道打包]  
categories: Android  
---

# 多渠道打包

Eclipse时代，结合ant也是可以进行多渠道打包的，可惜效率以及便捷性上面比现今的Gradle差了许多，Gradle通过可配置的方式来进行多渠道打包，可配置的元素有很多，例如：包名、资源、代码、版本号等等，灵活性很大，也很节省时间，常规方式默认打包构建时会生成各个渠道对应的代码和资源文件，然后逐个进行打包，这样子渠道很多时也是相对费时间的，也很耗费电脑性能。此外有一些方案，比如美团之前提到的多渠道打包，首个APK完整构建后，其他的会根据首个构建好的APK进行资源和代码的替换来生成新的渠道包，以这种方式来操作，100个渠道包也不需要太多时间的，后续会对该种方式进行补充记录。

下面记录一下实际操作用到的一些配置，打多渠道包一般分为两步：

- 配置渠道
- 配置渠道特有的信息

<!--more-->

## 配置渠道

在module的build.gradle进行各个渠道的声明：  

```Groovy
android {
   ...
    productFlavors {
        wandoujia {}
        qihu360 {}
        baidu {}
    }
}
```

## 渠道特有信息配置

eg.友盟的渠道配置一般写到manifest.xml里

```XML
<meta-data
	android:name="UMENG_CHANNEL"
	android:value="${CHANNEL_VALUE}" />
```

占位符在build.gradle里各个渠道声明处进行信息配置，defaultConfig也可以配置默认值

```Groovy
//渠道列表
productFlavors {
    wandoujia {
        //包名
		applicationId "com.example.wandoujia"

		//动态配置字符串
		resValue "string", "app_name", "豌豆荚专用"  
		
		//动态配置字符串到buildConfig类里方便在代码里引用            
		buildConfigField 'String','URI_JPUSH_HISTORY','"xxx"'
            
		//对占位符进行赋值，支持多个
		manifestPlaceholders = [
				JPUSH_APPKEY_VALUE: "xxx",
				JPushHistoryContentProvider_VALUE: "xxx",
				JPUSH_REVEIVER_CATEGORY_VALUE: "xxx",
				JPUSH_PERMISSION_VALUE: "xxx",
				APPKEY_VALUE: "xx",
				UMENG_APPKEY_VALUE: "xxx"
        ]
    }
}

//指定渠道名 flavor的name eg.wandoujia
productFlavors.all {
    flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
}
```

defaultConfig配置的清单文件（AndroidManifest.xml）的一些设置。defaultConfig的配置将覆盖AndroidManifest.xml中的设置。 
defaultConfig元素指定的配置适用于所有的版本(build variants)，除非一个build variants的配置将覆盖一些值。

```Groovy
defaultConfig {
    minSdkVersion 8
    targetSdkVersion 22
    versionCode 10
    versionName "3.0"

    // dex突破65535的限制
    multiDexEnabled true

    applicationId "com.cdel.chinaacc.exam.bank"
}
```

另外如果有一些渠道有特有的逻辑代码和资源也是可以配置的。在src/main 这里，main表示程序构建默认使用的代码和资源，我们可以在main同级目录建立和渠道名相同名字的文件夹，eg.src/wandoujia，下面的子目录需要和main下的结构一样，比如：

```Java
	src/wandoujia
		/assets
		/res
			/drawable
			/drawable-xhdpi
			/values
				/strings.xml
	src/main
```

这样在多渠道打包自动构建时，相应的渠道会自动到相应目录下使用渠道包自己的代码和资源来进行打包，有几个原则是：

- 代码和资源文件进行替换
- 字符串相关文件进行合并追加

这样一来，多渠道打包就变得灵活很多。

# 开发配置相关

## 保持旧的eclipse文件结构

```Groovy
	android {
	     sourceSets {
	       main {
	         manifest.srcFile 'AndroidManifest.xml'
	         java.srcDirs = ['src']
	         resources.srcDirs = ['src']
	         aidl.srcDirs = ['src']
	         renderscript.srcDirs = ['src']
	         res.srcDirs = ['res']
	         assets.srcDirs = ['assets']
	    }
	     androidTest.setRoot('tests')
	    } 
	}
```

在grade文件中配置，将会保存eclipse目录结构，当然，如果你有任何依赖的jar包，你需要告诉gradle它在哪儿，假设jar包会在一个叫做libs的文件夹内，那么你应该这么配置：

```Groovy
dependencies {
       compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

该行意为：将libs文件夹中所有的jar文件视为依赖包。

## Android-Gradle DSL

defaultConfig{ } 默认配置，是ProductFlavor类型。它共享给其他ProductFlavor使用  
sourceSets{ } 源文件目录设置，是AndroidSourceSet类型。  
buildTypes{ } BuildType类型  
signingConfigs{ } 签名配置，SigningConfig类型  
productFlavors{ } 产品风格配置，ProductFlavor类型  
testOptions{ } 测试配置，TestOptions类型  
aaptOptions{ } aapt配置，AaptOptions类型  
lintOptions{ } lint配置，LintOptions类型  
dexOptions{ } dex配置，DexOptions类型  
compileOptions{ } 编译配置，CompileOptions类型  
packagingOptions{ } PackagingOptions类型  
jacoco{ } JacocoExtension类型。 用于设定 jacoco版本  
splits{ } Splits类型。 

