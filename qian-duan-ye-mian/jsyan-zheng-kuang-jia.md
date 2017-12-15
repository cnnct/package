Js的form表单验证主要是基于jquery.validate.js插件框架，插件的版本为v1.14.0,
文件引用位置如下：
![](/assets/frontDoc_validate1.png)
Validate初始化加载的js在tag-src/js包下的common.js文件中内容如下为：
jQuery.fn.formValidate = function(obj) {
	var validator = $(this)
			.validate(
					{
						errorElement : 'div',
						errorClass : 'help-block',
						focusInvalid : false,
						ignore : "",
						rules : obj.rules,
						messages : obj.messages,
						highlight : function(e) {
							$(e).closest('.form-group').removeClass('has-info')
									.addClass('has-error');
						},

						success : function(e) {
							$(e).closest('.form-group')
									.removeClass('has-error');// .addClass('has-info');
							$(e).remove();
						},
						errorPlacement : function(error, element) {// 错误时添加提示
							if (element.is('.select2')) {
								error
										.insertAfter(element
												.siblings('[class*="select2-container"]:eq(0)'));
							} else if (element.is('select')) {// 选择框类型的标签验证错误
								error.insertAfter(element);
							} else if (element.is('.chosen-select')) {
								error
										.insertAfter(element
												.siblings('[class*="chosen-container"]:eq(0)'));
							} else if (element.is('.cust_file_upload')) {// 文件类型的标签验证错误
								error.insertAfter(element.parents('span'));
							} else {
								error.insertAfter(element.parent());
								if (element.is('input[type=checkbox]')
										|| element.is('input[type=radio]')) {
									element
											.parent("label")
											.next(".help-block")
											.attr("style",
													"top: 20px; left: 100px; display: block;");
								}
							}
						},

						submitHandler : function(form) {
							form.submit();
						},
						invalidHandler : function(form) {
						}
					});
	return validator;

}

该方法返回validator对象
配合该js验证的自定义表单验证，正则表达式验证，以及自定义方法验证的初始化加载js在tag-src/js包下的common.js文件中，内容如下：
function customValidateRun() {

	for ( var i in customValidate.validateVal) {
		var message = customValidate.validateVal[i].message;
		// 如果是正则表达式验证
		if (customValidate.validateVal[i].regEx != undefined) {
			jQuery.validator.addMethod(i, function(value, element, param) {
				var regEx = customValidate.validateVal[param].regEx;
				return this.optional(element) || (regEx.test(value));
			}, message);
		} else if (customValidate.validateVal[i].method != undefined) {// 方法验证
			jQuery.validator.addMethod(i, function(value, element, param) {
				var func = customValidate.validateVal[param].method;
				var flag = func(value);
				// 创建函数对象，并调用,将对象中callbackData数据传回
				return this.optional(element) || flag;
			}, message);
		}
	}
}

该方法将加载resource资源包下的custom_validate.js文件中的内容，
custom_validate.js为开放给开发人员的文件，主要可以按规则加入验证的正则表达式和自定义的验证方法，相关的内容如下：


* Form表单验证的清空：
表单验证配合的表单清空js方法在tag-src/js包下的common.js文件中，代码内容如下：
// 清空form表单
function formClean($form) {
	// 清除基础对象和验证的相关内容
	$form.validate().resetForm();
	// 清除所有hidden的元素的内容
	var $hiddens = $form.find("input[type=hidden]");
	$.each($hiddens, function(i) {
		$hiddens.eq(i).val("");
	})
	// 清除树对象
	var $ztrees = $form.find(" ul.ztree");
	$.each($ztrees, function(i) {
		var treeId = $ztrees.eq(i).attr("id");
		var treeObj = $.fn.zTree.getZTreeObj(treeId);
		treeObj.checkAllNodes(false);
	})
	// 将多选select重置为--请选择--
	$form.find(".multiselect-selected-text").html("--请选择--");
	$form.find(" button.multiselect").attr("title", "--请选择--");
	var $activeLis = $form.find(" ul.multiselect-container li.active");
	$.each($activeLis, function(i) {
		$activeLis.eq(i).removeClass("active");
	});
	// 清除错误状态
	var $elements = $form.find(" .has-error");
	$.each($elements, function(i) {
		$elements.eq(i).removeClass('has-error');
	})
	// 清除textarea
	var $textareas = $form.find("textarea");
	$.each($textareas, function(i) {
		$textareas.eq(i).html("");
	})
	// 清除文件上传
	var $files = $form.find(".file_mult_upload_data");
	$.each($files, function(i) {
		var fileId = $files.eq(i).attr("id");
		var fileData = $("#" + fileId).data();
		file_mult_upload(fileData);
	});
	// 清除ueditor中的内容
	var $ueditorArea = $form.find(".ueditor-group textarea");
	$.each($ueditorArea, function(i) {
		var ueditorId = $ueditorArea.eq(i).siblings('div').attr("id");
		UE.getEditor(ueditorId).setContent('');
	});

	// 初始化双向列表
	var $removeallBtn = $(".bootstrap-duallistbox-container .removeall");
	$.each($removeallBtn, function(i) {
		$removeallBtn.eq(i).click();
	})

}
表单清空需要清空验证的validator对象，以及需要手动清空的封装对象。
![![](/assets/frontDoc_validate1.pn](/assets/frontDoc_validate2.png)g)