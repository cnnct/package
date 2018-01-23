# 皮肤切换
* 涉及的js，css文件：
![](/assets/frontDoc_style2.png)
* style_index.ftl主要页面代码：

		<@init_page title="风格切换">
		 <@form id="style_form">
	        <@form_group class="row">
				<div class="styleSkin">
					<h4>请选择风格：</h4>
					<ul id="skin">
						<li id="skin_blue" title="蓝色">蓝色</li>
						<li id="skin_green" title="淡绿色">淡绿色</li>
						<li id="skin_orange" title="橙色">橙色</li>
						<li id="skin_skyblue" title="天蓝色">天蓝色</li>
						<li id="skin_red" title="红色">红色</li>
					</ul>
				</div>
				<@span id="style_span" value="" size="12"/>
	        </@form_group>
	    </@form>			
		<script>
			$(function(){
				var $li = $("#skin li");
				$li.click(function(){
					switchSkin(this.id);
				});
				
				//初始化加载时勾选中当前的风格样式
				var styleUrlStr=$("#init_cssfile").attr("href");
				var iIndex=styleUrlStr.indexOf(".",styleUrlStr.lastIndexOf("/"));
				//截取风格样式
				var styleType=styleUrlStr.substring(styleUrlStr.lastIndexOf("/")+1,iIndex);
				$("#"+styleType).addClass("selected");
			});
			//切换风格
			function switchSkin(skinName){
				$("#"+skinName).addClass("selected").siblings().removeClass("selected"); //当前<li>元素选中并去掉其他的元素
				//保存风格样式
				//获取选中的风格id
				var styleId=$("#skin .selected").attr("id");
				if(styleId!=undefined){
					 $.ajax({
			                type : 'POST',
			                url : '${base}/saveStyle',
			                dataType : 'json',
			                data : {
			                    styleType:styleId
			                },
			                success : function(data) {
								window.parent.$("#cssfile").attr("href","${base}/assets/css/skin/"+skinName+".min.css");//设置index.ftl首页皮肤
								if(data.resultCode==0){
				                	$("#style_span").html("界面风格保存成功，部分内容刷新界面后生效");
								}else{
									$("#style_span").html("界面风格保存失败");
								}
			                },
			                error : function() {
			                	$("#style_span").html("界面风格保存失败");
			                }
					 });
				}
			}
		</script>
		</@init_page>
	