[TOC]

## Android代码混淆

### 1.使用proguard混淆android代码

#### （1）Proguard基本语法

保留类名

```
-keep class * implements Object{ // 保留继承至Object中的类名不被混淆
	
}
```

保留方法名

```
-keepclassmembers public class * extends Object{ // 保留继承至Object的类的get和set方法不被混淆 
	void set*(***)
	*** get*();
}
```

保留native类的类名和方法名

```gradle
-keepclasswithmembernames class * { // 保留native的类名及方法名不被混淆
		native <methods>
}
```

保留类名和方法名

```proguard
-keepclassmembers class * extend Object{ // 保留继承至Object的类的参数为View方法不被混淆
	public void *(android.view.View) 
}
```

#### （2）在哪里去控制是否使用proguard混淆呢？

module --> build.gradle--> buildTypes--> release --> minifyEnabled 

minifyEnabled为false则为不使用混淆；minifyEnabled为true则为使用混淆

#### （3）怎么判断proguard是否混淆成功了呢？

第一步：./gradlew assemblerelease  编译工程 生成apk后

第二部：使用反编译工具打开该apk文件。查看混淆情况

#### （4）那么android studio完成混淆的混淆文件的基本语法定义在哪两个文件中呢？

```android
module --> build.gradle--> buildTypes--> 
proguardFiles getDefaultProguardFile('proguard-android.txt'),'proguard-rules.pro'
```

其中proguard-android.txt在android sdk目录下tools/proguard/proguard-android.txt下，这里定义了proguard的基本语法，在下载sdk的时候就已经存在了

而proguard-rules.pro文件就是我们自定义混淆规则的地方

### 2.使用proguard去除日志信息

#### （1）我们为什么要使用proguard去除日志信息

在我们开发的时候需要日志信息，但在上线release包中我们希望不再输出日志

#### （2）如何通过proguard去除日志信息呢

通过配置proguard ，将android.util.Log类的方法置为无效代码，可以去除apk中的打印日志的代码

直接删除打印log代码

```
-assumenosideeffects class android.util.Log{
	public static boolean isLoggable(java.lang.String,int);
	public static void v(...);
	public static void i(...);
	public static void w(...);
	public static void d(...);
	public static void e(...);
}

-assumenosideeffects class com.example.log.LogUtil{
	public static void v(...);
	public static void i(...);
	public static void w(...);
	public static void d(...);
	public static void e(...);
}

```

#### (3)混淆文件选择

'proguard-android.txt'替换为'proguard-android-optimize.txt'

