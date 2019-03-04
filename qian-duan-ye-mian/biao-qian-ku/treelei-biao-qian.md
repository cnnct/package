# ！！！更新后的变化，以及后面增加的可编辑的tree

# tree类标签

* 主要涉及文件：
  加载tree，input\_tree类组件需要的文件有：tree.ftl,input\_tree.ftl,TreeTag.java,InputTree.java,jquery.ztree.all-3.5.js,common.js等
* 主要加载：
  在tree加载时将参数传入，模板读取参数初始化tree对象，common.js中涉及到初始化tree的setting内容：
  ![](/assets/frontDoc_tree1.png)
  
* 重点介绍一下tree开启可编辑功能：
  
  1.当页面加载tree组件要求开启可编辑功能时，即开启edit_flag属性，必须也要有url属性来加载tree的数据，此时其他两种（通过sql_key加载数据和自定义后台加载数据）加载数据的方式会失效
  ![](/assets/tree_edit_1.png)
  



