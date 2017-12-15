此处理可写一下总体ehcache配置，特别是负载配置

#### ehcache.xml

```
    <!-- ================负载配置开始=============== -->
    <!-- 
	除配置本段两个Factory节点外，还需要在各子缓存的结点中增加cacheEventListenerFactory，详见sysPara示例配置部分
	共分为①、②、③，共三处配置
	
	负载配置①：配置对方缓存服务信息
	peerDiscovery：atutomatic自动，manual手动，本例使用manual
	rmiUrls：对方缓存服务url，本例中只是以sysPara为例，若是多个url时，中间用|竖线分隔即可
	timeToLive 搜索某个网段上的缓存：0是限制在同一个服务器，1是限制在同一个子网，32是限制在同一个网站，
		64是限制在同一个region，128是同一块大陆，还有个255
	!!!!!特别注意，我在本机测试两个tomcat时，一直不生效，后来加上timeToLive=0，即生效
	-->       
<!-- 	<cacheManagerPeerProviderFactory  -->
<!-- 		class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory" -->
<!-- 		properties="peerDiscovery=manual,timeToLive=0,rmiUrls=//127.0.0.1:40001/sysPara"/> -->
	
	<!-- 负载配置②：配置本地ip，端口
		hostName：主机名或者ip
		socketTimeoutMillis:同步超时时间
	 -->
<!-- 	<cacheManagerPeerListenerFactory  -->
<!--    		class="net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"  -->
<!--    		properties="hostName=127.0.0.1,port=40002,socketTimeoutMillis=2000"/>  -->
	<!-- ================负载配置结束=============== -->
```



