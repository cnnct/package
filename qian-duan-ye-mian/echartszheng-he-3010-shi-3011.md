##echarts整合：
* 1.echarts组件的整合，主要是基于第三方插件框架echarts-all.js实现，采用后台方式封装传输配置数据，后台封装采用第三方框架echart类库echarts-2.2.7.jar配合echart-all.js的版本，
第三方echart类库的api文档地址在：
https://oss.sonatype.org/content/repositories/releases/com/github/abel533/ECharts
* 2.echart标签组件的ftl模板内容和java内容如下：
java内容：

		 @Override
		    public void execute(Environment env, Map params, TemplateModel[] loopVars, TemplateDirectiveBody body)
		            throws TemplateException, IOException {
		    	//将要校验的属性加入数组
		    	try {
			        String [] array=new String[]{"id","size","name","class","position"};
			        //获取校验完的结果
			        Map<String, Object> map;
					map = TagUtil.getCheckedParams(params, array, "echarts");
					//加载option数据的url地址
					String optionUrl=FormaterUtils.checkObjToStr(params.get("option_url"));
					//获取定时器的加载时间
					String intervalTime=FormaterUtils.checkObjToStr(params.get("interval_time"));
					String intervalFlag="false";//定时器开关,默认关闭
					if(!"".equals(intervalTime)&&(Integer.parseInt(intervalTime))>0){//如果不需要开启定时器
						intervalFlag="true";
					}
					//样式
					String style=FormaterUtils.checkObjToStr(params.get("style"));
					if("".equals(style)){
						style="height:400px;";
					}
			        //获取模板
			        Template template = TagUtil.getFtlTemplate(env,this.getClass(),"echarts.ftl" );
			        Writer out = env.getOut();
			        map.put("optionUrl", optionUrl);
			        map.put("intervalFlag", intervalFlag);
			        map.put("intervalTime", intervalTime);
			        map.put("style", style);
			        template.process(map, out);
			        out.flush();
		    	} catch (Exception e) {
		    		log.error("echarts标签加载失败："+e.toString());
		    		e.printStackTrace();
		    	}