session管理，包括redis如果启用时有何配置区别等

#### session缓存

```
    <!--session缓存，失效时间可设置为15分、20分等均可-->
    <!--由于session缓存、operSes缓存是一体的，所以失效时间必须设置为一致-->
    <cache name="session"
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
```



