##单文件上传：
* 1.单文件上传采用标签封装的组件方式，使用时，只要引用file_upload组件，单文件上传是在原生的文件类型的input标签上按照bootstrap风格封装了一层
* 2.模板ftl内容如下：
<div class="form-group">
	<#if labelHave=="true">
		<div class="${labelSize}">
		 	<label  class="form-label">${labelName}
			<#if  mustStar=="true">
				<span class="must_star">*</span>
			</#if>
			</label>
		</div>
	</#if>
	 <div class="form-group ${size}">
	    <div class="input-group">
	       <input type="text" class="form-control" id="${id}show"/>
	      <#-- 
	       <span class="input-group-btn">
	          <button class="btn btn-default" type="button" id="${id}btn" onclick="fileTrggier('${id}')">选择文件</button>
	       </span>
	       <input type="file" style="display:none" class="form-control" ${idStr} ${nameStr} onchange="clearTime('${id}')"/>
	       <input type="hidden"  class="form-control" id="${id}time"/>
	       -->
		   <span class="input-group-btn">
		       <a style="position:relative;display: inline-block;padding: 1px 24px;background:#02abfe;font-size: 12px;overflow: hidden;line-height: 28px;text-align:center;color: #ffffff;border-radius: 2px;cursor: pointer;text-decoration: none;"  href="javascript:void(0);" >选择文件
		       <input style="position:absolute;left:0;top:0;filter:alpha(opacity=0);opacity:0;cursor: pointer;" type="file"  class="form-control cust_file_upload" ${idStr} ${nameStr} onchange="setFileValue('${id}')"/>
		       </a>
	       </span>
	    </div>
	  </div>
	</div>
	<script>
		$(function() {
			<#--由于js改变值不能主动触发change事件来配合validate验证，所以需要绑定一下change事件-->
			$("#"+"${id}").change(function(){
				//获取父级form表单的id
				var formId=$("#"+"${id}").parents("form").attr("id");
				//获取父级form表单的validator对象
		 		var formValidator=$("#"+formId).validate();
		 		//验证单个元素
		 		formValidator.element($("#"+"${id}"))
	 		})
		});
	</script>

