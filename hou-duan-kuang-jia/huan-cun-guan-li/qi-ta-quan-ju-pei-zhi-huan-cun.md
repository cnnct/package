除session之外的其它结存的说明

#### 其它缓存 

```
<!-- 错误码缓存 ,暂定永不失效 -->
    <cache name="errCode"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
    	<searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
    <!-- code缓存  ,暂定永不失效  -->
    <cache name="sysCode"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
    	<searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
    <!-- syspara缓存  ,暂定永不失效  -->
   	<cache name="sysPara"
		   maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
    	<searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    	<!-- 负载配置③：配置RMI，非负载配置可注释 -->
<!--     	<cacheEventListenerFactory class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"/> -->
    </cache>

    <!--operid对应ses_id缓存-->
    <!--由于session缓存、operSes缓存是一体的，所以失效时间必须设置为一致-->
    <cache name="operSes"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>

   <!--actionLog缓存,失效时间设置短一点-->
    <cache name="actionLog"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="false"
           timeToIdleSeconds="300"
           timeToLiveSeconds="300"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
    <!--trcode缓存 ,暂定永不失效  -->
    <cache name="trCode"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
    <!--trOrder缓存 ,暂定永不失效  -->
    <cache name="trOrder"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
     <!--mapValue缓存,失效时间设置短一点-->
    <cache name="mapValue"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="false"
           timeToIdleSeconds="300"
           timeToLiveSeconds="300"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
    
     <!--reqCode缓存,失效时间设置短一点-->
    <cache name="reqCode"
            maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
    </cache>
```



