# 按后面优化的首页，特别是面板的实现

##首页布局：

1.介绍首页右侧主模块嵌入：
 * 在首页index.ftl或卡管首页index_card.ftl中嵌入主模块：
 ![](/assets/index_1.png)
 * 通过请求后台加载首页主模块嵌入页：
 ![](/assets/index_2.png)
 *  主要讲一下布局标签：
 布局模块panel_contiainer标签主要是根据表格的思想，将每行分成12块（bootstrap样式思想），将需要布局的面板由左往右，从上至下，包裹td和tr来实现布局。
 后台代码：
![](/assets/index_3.png)
 模板代码：
![](/assets/index_4.png)