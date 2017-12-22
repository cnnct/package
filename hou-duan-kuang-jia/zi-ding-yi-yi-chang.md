包括自定义异常  
异常拦载  
页面异常处理等

#### 自定义异常

主动抛异常时，使用自定义异常CustomException

抛出异常后，会被CustomExceptionResolver捕获，CustomExceptionResolver类通过实现HandlerExceptionResolver接口，重写resolveException方法处理自定义异常。在此方法中，会根据请求类型，浏览器类型等分别做处理。

如果是普通request请求，会跳转到错误页面，错误页面会根据此处存入的信息，判断是否显示错误页面。

