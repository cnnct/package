##级联组合标签：
1.级联标签组说明：主要指的是cas_select_parent,cas_select_child,input_tree的组合，
有两种组合方式：
（1）cas_select_parent + cas_select_child + cas_select_child + ......,即第一级用parent标签，后续级都用child标签；
（2）input_tree + cas_select_child + cas_select_child + ......,即第一级用input_tree标签，后续级都用child标签；

2.以上第一种组合说明：
* 页面加载级联组件，传入参数到后台：
![](/assets/cas_1.png)
* 根据传入的参数初始化加载组件：
parent标签：
![](/assets/cas_2.png)
child标签：
![](/assets/cas_3.png)
* 点击的时候加载（common.js）：
为parent-select和child-select属性绑定change事件：
![](/assets/cas_5.png)
![](/assets/cas_6.png)