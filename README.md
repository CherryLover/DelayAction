## DelayAction 目标方法前置检验模型

### 一、需求背景
在执行目标行为时，需要执行一些前置的行为。而这些前置行为，需要用户参与才能完成。例如：未登录情况下点击关注用户，跳转登陆，登陆成功后自动执行关注。


### 二、如何使用

- 无嵌套调用（常用场景，单Action）：

`ActionActivity`实现`Action`接口，或 new `Action`实现类，实现 call 目标行为。

```
SingleCall.getInstance()
          .addAction(ActionActivity.this)
          .addValid(new LoginValid())//前置条件，可能有多个
          .addValid(new OtherValid()
          .doCall();
```
前置行为完成后，调用`SingleCall.getInstance().doCall();`启动验证模型

- 嵌套调用（多Action）：
```
Call call1 = new Call(new Action() {
    @Override
    public void call() {
    }
});
Call call2 = new Call(new Action() {
    @Override
    public void call() {
    }
});
callUnit1.addValid(new LoginValid());
callUnit1.addValid(new AnotherValid());
callUnit2.addValid(new OtherValid());

MultipleCall.getInstance()
        .postCall(call1)
        .postCall(call2);
```
前置行为完成后，调用`MultipleCall.getInstance().reCheckValid();`启动验证模型

### 三、架构设计
![类关系图](https://upload-images.jianshu.io/upload_images/3167794-af92e0f102172d59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>[详细博文介绍请戳](https://www.jianshu.com/p/7114d5e82e8c)


> 本项目在 [jinyb09017](https://github.com/jinyb09017/delayActionDemo) 大大的基础上完善
> - 增加了容错处理
> - 补充了嵌套 Call 的情况

### 更新

以上全部内容来自原代码仓库：https://github.com/feelschaotic/DelayAction。

本人 fork 之后所做修改：

1. 将 build.gradle 文件中的 compile 更新为 implementation
2. 打包成第三方库。

#### 使用配置

1. 项目的根目录的 build.gradle 的 repositories 最后添加

```groovy
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```

2. 添加依赖

```groovy
dependencies {
  implementation 'com.github.CherryLover:DelayAction:lastVersion'
}
```

[![](https://jitpack.io/v/CherryLover/DelayAction.svg)](https://jitpack.io/#CherryLover/DelayAction)


