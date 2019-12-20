# redis单节点配置

###  properties 配置

```properties
#redis单机配置
redis.pool.host=172.20.25.14
redis.pool.port=7003
redis.pool.password=
redis.pool.maxIdle=8
redis.pool.maxActive=50
redis.pool.maxWait=1500
redis.pool.testOnBorrow=true
redis.pool.testOnReturn=true
redis.pool.database=1
```

### spring 配置

```xml
    <!-- 配置单机JedisPoolConfig-->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxIdle" value="${redis.pool.maxIdle}"/>
        <property name="maxTotal" value="${redis.pool.maxActive}"/>
        <property name="maxWaitMillis" value="${redis.pool.maxWait}"/>
        <property name="testOnBorrow" value="${redis.pool.testOnBorrow}"/>
        <property name="testOnReturn" value="${redis.pool.testOnReturn}"/>
    </bean>

    <bean class="redis.clients.jedis.JedisPool" id="jedisClient">
        <constructor-arg index="0" ref="jedisPoolConfig"></constructor-arg>
        <constructor-arg index="1" value="${redis.pool.host}"></constructor-arg>
        <constructor-arg index="2" value="${redis.pool.port}"></constructor-arg>
        <constructor-arg index="3" value="${redis.pool.maxWait}"></constructor-arg>
        <constructor-arg index="4" value="${redis.pool.password}"></constructor-arg>
        <constructor-arg index="5" value="${redis.pool.database}"></constructor-arg>
    </bean>
```

### java客户端使用

```java
    @Autowired
    private JedisPool jedisPool;

    @Override
    public boolean exists(String key) {
        boolean flag = false;
        Jedis jedis = null;
        try {
            jedis = jedisPool.getResource();
            flag = jedis.exists(protostuffSerializer.serialize(key));
        } catch (Exception ex) {
            logger.error("JedisService.exists 出错[key=" + key + "]", ex);
        } finally {
            closeResource(jedis);
        }
        return flag;
    }
```

