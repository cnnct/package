# ！！！！前后端结合说明，前端哪些控件或哪种场合可以使用，后端如何实现，涉及到的类、文件、表，等


##sql_key的加载：
1.使用场景说明：
sql_key主要是为了方便一些组件需要主动从数据库中动态获取数据而开发的，使用sql_key在后台数据库查找对应的执行语句而不是直接将语句写到前台也是为了安全起见，防止数据库信息暴露。

2.涉及的主文件和表：
![](/assets/sql_key_1.png)
表：sys_tag_sql

3.加载原理：
例：结合一个简单的例子说明（select组件）：
![](/assets/sql_key2.png)
当前台页面加载时，会进入到指定的后台加载类，判断存在sql_key,就会执行到后台查找key值对应的具体sql模板，然后结合传入的参数组装成完整的sql语句，最后在Common接口里执行获取数据返回渲染页面模板
![](/assets/sql_key3.png)










































，