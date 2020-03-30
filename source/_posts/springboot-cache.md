---
title: Spring Boot ---- 缓存相关
categories:
  - 笔记
tags:
  - Spring Boot
---

### Cache 缓存

**引入缓存依赖**

```xml
<!-- 引入 Spring 缓存依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**注解**

| 名称           | 解释                                           |
| -------------- | ---------------------------------------------- |
| Cache          | 缓存接口，定义缓存操作                         |
| CacheManager   | 缓存管理器，管理各种缓存组件                   |
| @Cacheable     | 缓存注解（方法调用前生效)                      |
| @CacheEvict    | 清空缓存（默认方法调用后）                     |
| @CachePut      | 调用方法后更新缓存                             |
| @EnableCaching | 开启基于注解的缓存（在SpringBoot启动类上使用） |
| @CacheConfig   | 统一配置本类的缓存注解的属性（类前使用）       |
| @Caching       | 组合多个Cache注解                              |
| keyGenerator   | 缓存数据时key生成策略                          |
| serialize      | 缓存数据时value序列化策略                      |

**<code>@Cacheable/@CacheEvict/@CachePut</code> 主要参数**

| 名称                           | 解释                                          |
| ------------------------------ | --------------------------------------------- |
| cacheNames/value               | 缓存空间名称（可指定多个）                    |
| key                            | 使用SpEL表达式指定key规则（默认组合所有参数） |
| condition                      | 缓存条件（true则缓存）                        |
| unless                         | 否定缓存条件（true则不缓存）                  |
| allEntries(@CacheEvict )       | 是否全部清空（默认false）                     |
| beforeInvocation(@CacheEvict ) | 执行方法前清空（默认false）                   |

**SpEL 上下文数据**

| 名称         | 位置   | 描述          | 示例                |
| ------------ | ------ | ------------- | ------------------- |
| methodName   | root   | 方法名        | #root.methodName    |
| method       | root   | 方法          | #root.method.name   |
| target       | root   | 对象实例      | #root.target        |
| targetClass  | root   | 对象的类      | #root.targetClass   |
| args         | root   | 参数列表      | #root.args[0]       |
| caches       | root   | 缓存列表      | #root.cache[0].name |
| argumentname | 上下文 | 参数名或a0/p0 | #id、#a0、#p0       |
| result       | 上下文 | 返回值        | #result             |

> <code>root</code> 对象可省略，<code>Spring</code> 默认使用 <code>root</code> 对象的属性

### 整合 Redis

> <code>redis</code> 默认安装完毕

**引入依赖**

~~~xml
<!-- 引入 Redis 依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

**配置redis**

~~~yml
spring:
	redis:
		host:xxx.xx.xx.xxx
~~~

**编写测试类**

~~~java
@SpringBootTest
public class Test {
    @Autowired
    UserMapper userMapper;
    
    @Autowired
    StringRedisTemplate stringRedisTemplate; // k-v都是字符串的
    
    @Autowired
    RedisTemplate redisTemplate; // k-v都是对象
    /**
     * 常见数据类型
     * String、list、set、hash、zset
     * stringRedisTemplate.opsForValue() ---- 字符串
     * stringRedisTemplate.opsForList() ---- 列表
     * stringRedisTemplate.opsForSet() ---- 集合
     * stringRedisTemplate.opsForHash ---- 散列
     * stringRedisTemplate.opsForZSet ---- 有序集合
     */
    @Test
    public void test() {
        /**
         * stringRedisTempalte.opsForValue().xxx();
         */
        // 保存
        stringRedisTemplate.opsForValue().append("mas", "hello");
        // 取出
        stringRedisTemplate.opsForValue().get("msg");
    }
}
~~~

> <code>Redis</code> 命令参考[Redis 命令](http://www.redis.cn/commands.html#)