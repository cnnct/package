# ！！！哪里增加公共方法，如何调用

1、实现方式：在LoginCtrl类中添加了validateUser方法，在global.js中添加了validateUser方法：

![](/assets/validateUser1.png)![](/assets/validateUser2.png)2、调用方法：在页面ajax提交数据到后台之前，调用validateUser\(\)方法即可。

