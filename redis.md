Redis调用

### 调用
具体见 [Redis 调用](https://segmentfault.com/a/1190000009695841)

// 添加队列
Redis::sadd('coupon_read1', '1');
Redis::sadd('coupon_read1', [2,3,4]);

// 删除队列
Redis::srem('coupon_read1', 1);
Redis::srem('coupon_read1', [2,3,4]);

// 弹出队列首元素
Redis::spop('coupon_read1');

// 获取队列
Redis::smembers('coupon_read1');

// 移动当前set集合的指定元素到另一个set集合
Redis::smove('set1', 'set2', 'ab');

//返回当前set表元素个数
Redis::scard('set2'); // 返回 2

// 返回表中一个随机元素
Redis::srandmember('set1') ;

// 返回两个表中元素的交集/并集/补集
Redis::sinter('set2', 'set1') ;  //返回array('ab')

// 将两个表交集/并集/补集元素 copy 到第三个表中
Redis::set('foo', 0);
// 等同于将'set1'的内容copy到'foo'中，并将'foo'转为set表
Redis::sinterstore('foo', 'set1');
// 将'set1'和'set2'中相同的元素 copy 到'foo'表中, 覆盖'foo'原有内容
Redis::sinterstore('foo', array('set1', 'set2'));

// 判断元素是否属于当前set集合
Redis::sismember('set2', '123'); // 返回 true or false
