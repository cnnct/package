## ！！！后来为实现宁波的图表是不是做了些优化，涉及到哪些文件、配置或数据

## echarts整合：

* 1.echarts组件的整合，主要是基于第三方插件框架echarts-all.js实现，采用后台方式封装传输配置数据，后台封装采用第三方框架echart类库echarts-2.2.7.jar配合echart-all.js的版本，
  第三方echart类库的api文档地址在：
  [https://oss.sonatype.org/content/repositories/releases/com/github/abel533/ECharts](https://oss.sonatype.org/content/repositories/releases/com/github/abel533/ECharts)
* 2.echart标签组件的ftl模板内容和java内容如下：  
  （1）java内容：

  ```
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
  ```

（2）ftl模板内容：

```
<div class="form-group ${size} ${positionStr} ">
    <div id="${id}" style="${style}"></div>
</div>
<script>
    <#--获取echart对象-->
    var ${id}_echart = echarts.init(document.getElementById('${id}')); 
    <#--echart的option配置项-->  
    var ${id}_option;
    <#--定时器-->
    var ${id}_echartInterval; 
    <#--当需要开启定时器时启动-->
    <#if intervalFlag=="true">
        ${id}_echartInterval = setInterval(function(){
        <#--通过后台获取option配置参数-->
        $.ajax({
                type : 'POST',
                url : '${optionUrl}',
                dataType : 'json',
                success : function(data) {
                     ${id}_option=JSON.parse(data.option);
                     ${id}_echart.setOption(${id}_option);   
                },
                error : function() {
                    alert("系统异常，加载失败");
                },
            });
        },${intervalTime?number});

    <#else>
        <#--不需要启动定时器>
        <#--通过后台获取option配置参数-->
        $.ajax({
                type : 'POST',
                url : '${optionUrl}',
                dataType : 'json',
                success : function(data) {
                     ${id}_option=JSON.parse(data.option);
                     ${id}_echart.setOption(${id}_option);   
                },
                error : function() {
                    alert("系统异常，加载失败");
                },
            });
    </#if>
</script>  
```

（3）调用后台获取option的配置：  
示例如下：

```
     //测试echarts
    @RequestMapping("/echarts")
    @ResponseBody
    public  Map<String,Object> echartsTest(String test){//以map方式返回
        Option option = new Option();
        option.title("某楼盘销售情况", "纯属虚构");
        option.tooltip().trigger(Trigger.axis);
        option.legend("意向", "预购", "成交");
        option.toolbox().show(true).feature(Tool.mark,
                Tool.dataView,
                new MagicType(Magic.line, Magic.bar, Magic.stack, Magic.tiled),
                Tool.restore,
                Tool.saveAsImage).padding(20);
        option.calculable(true);
        option.xAxis(new CategoryAxis().boundaryGap(false).data("周一", "周二", "周三", "周四", "周五", "周六", "周日"));
        option.yAxis(new ValueAxis());

        int sdf=(int)(Math.random()*100);
        int sdk=(int)(Math.random()*100);
        Line l1 = new Line("成交");
        l1.smooth(true).itemStyle().normal().areaStyle().typeDefault();
        l1.data(sdf, 12, sdk, 54, 260, 830, 710);

        Line l2 = new Line("预购");
        l2.smooth(true).itemStyle().normal().areaStyle().typeDefault();
        l2.data(sdf+100, sdk+100, 434, 791, 390, 30, 10);

        Line l3 = new Line("意向");
        l3.smooth(true).itemStyle().normal().areaStyle().typeDefault();
        l3.data(sdf+400, sdk+600, 601, 234, 120, 90, 20);

        option.series(l1, l2, l3);

        Map<String,Object> dataMap = new HashMap<String,Object>();
        String optionStr = GsonUtil.format(option);
        dataMap.put("option", optionStr);
        return dataMap;
    }
```
3.升级成3.0版本的echarts：
 * 为了能够兼容以前的2.0版本，所以新加了一个组件echarts3标签来实现升级后的版本，3.0版本相对于2.0版本渲染效果更好，注意2.0版本和3.0版本由于涉及到的jar包不一样所以不能混用，以下是echarts3的版本加载过程：



