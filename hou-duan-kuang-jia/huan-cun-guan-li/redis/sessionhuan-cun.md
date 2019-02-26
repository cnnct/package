#### session缓存

如果启用了redis，那么session缓存将不再存入ehcache，而是redis。

redis缓存需要配置redis服务器，存于硬盘上，所以所存对象必须序列化。

