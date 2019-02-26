# ！！！太简单了，应能说明关联的类，页面的ajax的异常，以及普通request异常等，异常是如何通过哪几个类和页面协作，并返回！！！

包括自定义异常  
异常拦载  
页面异常处理等

#### 自定义异常

主动抛异常时，使用自定义异常CustomException

抛出异常后，会被CustomExceptionResolver捕获，CustomExceptionResolver类通过实现HandlerExceptionResolver接口，重写resolveException方法处理自定义异常。在此方法中，会根据请求类型，浏览器类型等分别做处理。

如果是普通request请求，会跳转到错误页面，错误页面会根据此处存入的信息，判断是否显示错误页面。

##### ajax请求和普通request请求分别处理：

![](/assets/CustomException1.png)

##### ajax请求，将异常通过ResultData类实例化，转换成JSON格式的数据，传到页面：

![](/assets/CustomException3.png)

##### request请求，直接跳转到错误页面：

![](/assets/CustomException2.png)

