# 级联类标签
* 涉及到的主要文件：

   cas_select_child.ftl,cas_select_parent.ftl,CasSelectChildTag.java,CasSelectParentTag.java,
   TagCtrl.java,common.js等
* 加载：
在初始化组件的时候绑定change方法，来触发每次改变值的数据处理：
		$('.parent-select').change(function() {
				// 获取option的选中值
				var optionval = $(this).find("option:selected").val();
				var childinfo = $(this).siblings('.child-info').data();
				childinfo = JSON.stringify(childinfo);
				$.ajax({
					type : 'POST',
					url : base + '/tag/casPanrentChange.do',
					dataType : 'json',
					// beforeSend : function(XMLHttpRequest) {
					// load();
					// },
					data : {
						optionval : optionval,
						childinfo : childinfo
					},
					success : function(data) {
						checkChild(data);
						// load('close');
					},
					error : function() {
						// load('close');
						alert("系统异常，加载失败");
					},
				});
			});
			// 级联下拉框子级触发
			$('.child-select').change(function() {
				var flag = $(this).siblings('.child-info').length;
				if (flag) {
					// 获取option的选中值
					var optionval = $(this).find("option:selected").val();
					var childinfo = $(this).siblings('.child-info').data();
					childinfo = JSON.stringify(childinfo);
					$.ajax({
						type : 'POST',
						url : base + '/tag/casPanrentChange.do',
						dataType : 'json',
						// beforeSend : function(XMLHttpRequest) {
						// load();
						// },
						data : {
							optionval : optionval,
							childinfo : childinfo
						},
						success : function(data) {
							checkChild(data);
							// load('close');
						},
						error : function() {
							// load('close');
							alert("系统异常，加载失败");
						},
					});
				}
			});   