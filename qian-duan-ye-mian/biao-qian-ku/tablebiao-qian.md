#表格组件
* 涉及的主要文件：
 
 加载表格涉及到的主要文件有
 table.ftl,table_toolbar.ftl,TableTag.java,jquery.dataTables.js,jquery.dataTables.bootstrap.js,pagination.js,dataTables.buttons.js,dataTables.tableTools.js,table_tag.js,
* 表格加载过程：
在页面加载表格自定义标签的时候，table.ftl会根据传入的参数初始化表格的数据，表格数据的获取主要根据table.ftl的ajax项参数进行数据加载，ajax从后台获取完数据后调用dataSrc回调函数进行回调处理相关数据 
![](/assets/frontDoc_table1.png)
![](/assets/frontDoc_table2.png)
![](/assets/frontDoc_table3.png)
后台对表格加载数据的封装主要在BaseC.java