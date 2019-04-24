# **页面使用：**print**标签**

#### 相关文件目录：/ocx/print

#### print**标签的属性 :**

> init\_page标签为初始化加载标签，有3个属性分别为title、link、script
>
> > **id ：** 对应上级模态框的id，若非模态框打开的打印窗口，可为空
> >
> > **modaliframe ：**以模态框打开打印窗口时，值为true，默认不指定时为false，用于返回按钮判断关闭窗口方法时使用
> >
> > **badkurl ：** 用于非模态框打开打印窗口时，点击返回按钮返回原页面的url
>
> ```
> <@print backurl="${base}${resultData.backurl}" id="modal_print" modaliframe="true"> 
>     <table>
>         <tr> 
>             <td><b>姓名:</b>${resultData.perName}</td> 
>             <td><b>性别:</b>${resultData.sex}</td> 
>         </tr>
>         <tr>
>             <td><b>证件号:</b>${resultData.certNO}</td> </tr> <tr> 
>             <td><b>手机号:</b>${resultData.mobile}</td> 
>         </tr>
>     </table>
> </@print>
> ```
>
> ![](/assets/print.png)

# 后端配合**使用：保存report表，组装打印显示数据**

> Ctrl代码
>
> ```
> JSONObject jsonObject = cardServiceServ.getSysReportContent(Long.valueOf(actionNo));
> model.addAttribute("jsonObject", jsonObject);
> return "/counter/cardservice/loss_reloss_print";
> ```
>
> Service代码
>
> 调用baseServImpl.saveSysReport方法保存report



