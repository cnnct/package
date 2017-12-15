##js提示对话框
* 1.js弹出框的封装，主要基于第三方框架bootbox.js,版本为v4.4.0,该框架基于bootstrap.js框架
* 2.封装的js代码在tag-src/js包下的common.js，内容如下：
	
		// alert弹框方法
		function alert(obj) {
			if (obj != undefined && obj != "") {
				var message = "";
				var title = obj.title;
				if (Object.prototype.toString.call(obj) == "[object Object]") {// 对象类型，排除数组等,包含element对象和普通对象
					if (typeof (obj) == "object" && obj.length > 0) {// 如果传入的obj为element对象
						var elementId = obj.attr("id");// 获取对象的id属性值
						var elementName = obj.attr("name");// 获取对象的name属性值
						message = "element:" + obj.get(0).tagName;
						if (elementId != undefined) {// 拼接id信息
							message = message + ";Id:" + elementId;
						}
						if (elementName != undefined) {// 拼接name信息
							message = message + ";Name:" + elementName;
						}
					} else if (obj.message != undefined) {// 有指定message
						message = obj.message;
					} else if (typeof (obj) == "object" && obj.message == undefined) {// 普通对象
						message = toString.call(obj);
					}
				} else if (Object.prototype.toString.call(obj) == "[object String]") {// 字符类型
					message = obj;
				} else {// 其他类型
					message = Object.prototype.toString.call(obj);
				}
		
				if (obj.title == undefined) {
					title = "提示";
				}
				bootbox.alert({
					title : title,
					message : message,
					size : "small",
					callback : obj.callback,
					buttons : {
						ok : {
							label : obj.okBtnName,
						}
					},
				});
			}
		}
		// confirm弹框方法
		function confirm(obj) {
			if (obj != undefined) {
				if (obj.title == undefined) {// 如果title不存在，则默认title为提示
					bootbox.confirm({
						title : "提示",
						size : "small",
						message : obj.message,
						callback : obj.callback,
						buttons : {
							confirm : {
								label : obj.okBtnName,
							},
							cancel : {
								label : obj.cancelBtnName,
							}
						},
					});
				} else {
					bootbox.confirm({
						title : obj.title,
						size : "small",
						message : obj.message,
						callback : obj.callback,
						buttons : {
							confirm : {
								label : obj.okBtnName,
							},
							cancel : {
								label : obj.cancelBtnName,
							}
						},
					});
				}
			}
		}
		// prompt弹框方法
		function prompt(obj) {
			if (obj != undefined) {
				if (obj.title == undefined) {// 如果title不存在，则默认title为提示
					bootbox.prompt({
						title : "提示",
						size : "small",
						callback : obj.callback,
						buttons : {
							confirm : {
								label : obj.okBtnName,
							},
							cancel : {
								label : obj.cancelBtnName,
							}
						},
					});
				} else {
					bootbox.prompt({
						title : obj.title,
						size : "small",
						callback : obj.callback,
						buttons : {
							confirm : {
								label : obj.okBtnName,
							},
							cancel : {
								label : obj.cancelBtnName,
							}
						},
					});
				}
			}
		}
		// loading加载提示弹框=======================================================================================================================================开始//
		function load(option) {
			if (option == 'close') {
				$(".bootbox-close-button").click();
				$(".bootbox-close-button").show();
		
			} else if (option != undefined) {
				bootbox.dialog({
					size : "small",
					message : '<div class="text-center"><img  src="' + base
							+ '/assets/img/loading1.gif" height="25" width="25" /> '
							+ option + '...</div>'
				});
				$(".bootbox-close-button").hide();
			} else {
				bootbox
						.dialog({
							size : "small",
							message : '<div class="text-center"><img  src="'
									+ base
									+ '/assets/img/loading1.gif" height="25" width="25" /> 加载中...</div>'
						});
				$(".bootbox-close-button").hide();
			}
		}
		