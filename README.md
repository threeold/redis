# redis基本用法

### 1、定义的key，“：”隔开目录层级关系;
$redis_key="ads:banner";

### 2、要缓存的值
$redis_val="123a";

### 3、设置缓存，不过期
$redis_info = \RedisDB::set($redis_key,redis_val);

### 4、缓存时间
$cache_time=86400;
 
### 5、设置缓存，过期时间86400秒后
\RedisDB::setex($redis_key, $cache_time, $redis_val);
 
### 6、设置读取缓存
$redis_info = \RedisDB::get($redis_key);

### 7、分布式锁 返回整数 1成功  0不处理
$isupdate = \RedisDB::setnx($redis_key,$redis_val);

### 8、删除缓存
\RedisDB::del($redis_key); 

## 订阅 10、11
/**
  'redis_config' => [
            'host'     => '127.0.0.1',
            'port'     =>6379,
            'database' => 0,
            'password' => "",
            'read_write_timeout'=> -1
        ],**/
        
### 10、发布订阅
\RedisDB::connection('redis_config')->publish($redis_key, $redis_val); 

### 11、psubscribe命令订阅一个或多个符合给定模式的频道
 $redis_key2="ads:banner2";
 \RedisDB::psubscribe([$redis_key,$redis_key2], function ($message) use ($that) {
                    
                        dump($message);//接收到的信息
                        
                    }, "redis_config");

## 队列

### 13、将值插入到列表头部
\RedisDB::lpush($redis_key, $redis_val); 

### 移除列表的最后一个元素，返回值为移除的元素。
 $redis_info= \RedisDB::rpop($redis_key);
 
 
## 哈希(Hash) 

 $redis_key="users:banner";//定义的key;
 
 $uid=1; //用户ID
 
### 14、命令用于查看哈希表的指定字段是否存在 
 $is_exis = \RedisDB::hexists($redis_key, $uid); 
 
### 15、命令用于为哈希表中的字段赋值 
 \RedisDB::hset($redis_key, $uid, $redis_val); 
 
### 16、命令用于返回哈希表中指定字段的值。 
 $res_info = \RedisDB::hget($redis_key, $uid);
 
 
 ## Set 无序集合
 ###String 类型的无序集合,集合成员是唯一的，这就意味着集合中不能出现重复的数据。
 
 $redis_key="users:test";//定义的key;
 
 ### 命令将成员元素加入到集合中key 值
 $is_exis = \RedisDB::SADD($redis_key, $uid);  

 
 ## sorted set 有序集合
 ### 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。
 
 不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。 
 ### 命令将成员元素加入到集合中key 分数，值
  $isupdate = \RedisDB::ZADD($redis_key,$grade, $uid);  
  
 
 
 
 
  
//其他的命令请去看菜鸟教程
