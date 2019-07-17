### 数据库调试

开启日志
```
\DB::enableQueryLog();
```
获取日志
```
\DB::getQueryLog();
```

### 原生SQL

```
whereRaw()
orderBy('关系表.字段', '排序')
orderByRaw()
havingRaw()
\DB::raw()
// 用MYSQL自带的函数
selectRaw()
User::select(DB::raw('sum(name),id'))->groupBy(DB::raw('find_in_set(xxxx,xxxx)'))->get();
->whereRaw('price > IF(state = "TX", ?, 100)', [200])
->whereRaw('orders.user_id = users.id');
方法:
getXxxAttribute(); // 修改器
setXxxAttribute(); // 修改器
scopeXxx(); // 查询器
```

条件语句
```
Order::when($type, function ($query, $type) {
  $query->whereType($type);
}, function ($query) {
  $query->whereType(0);
})->get();
```
Trait
```
class WhenTypeTrait
{
  public function scopeWhenType($query, $type)
  {
    $query->when($type, function ($query, $type) {
      $query->whereType($type);
    }, function ($query) {
      $query->whereType(0);
    });
  }
}
use WhenTypeTrait;
class Order extends Model
{
  use WhenTypeTrait;
}
Order::whenType($type)->get();
```

where
```
where字段($变量); => where(字段, '=', $变量);
PS:其实key,date等关键字不行特此说明
replicate() // 复制一行
```

withCount
```
问题:必须先select字段再withCount
指定别名
withCount('order as order_count' => function ($query) {
})

with // 预加载指定关联
    User::with('xxx:id,name', 'xxx:id,name,sort');
    User::with(['xxx:id,name', 'xxx' => function($query) {
        $query->select('id', 'name');
    }])
```

increment(字段, 增量,默认1);自增

decrement(字段, 减量,默认1);自减
```
// 设置
public static function boot()
{
  parent::boot();
  // 创建的时候执行
  self::creating(function ($model) {
    $model->uuid = (string)Uuid::generate();
  });
  // 更新的时候执行
  self::updating(function ($model) {
    // 写日志覆盖属性
  });
}

User::whereDate('created_at', date('Y-m-d'));
User::whereDay('created_at', date('d'));
User::whereMonth('created_at', date('m'));
User::whereYear('created_at', date('Y'));
whereHas // 条件
whereDoesntHave // whereHas相反函数
```

```
属性
    primaryKey // 主键默认 id
    keyType // 主键类型 默认int
    perPage // 定义分页每页显示数量（默认15）
    incrementing // 主键是否自增 默认是
    const CREATED_AT = 'created_at';
    const UPDATED_AT = 'updated_at';
    timestamps // 设置不需要维护时间字段
    fillable // 白名单 protected $fillable = [];
    guarded // 黑名单 protected $guarded = [];
    appends // 扩展访问器 protected $appends = []; $this->setAppends([]);
    dates // 日期转换器 protected $dates = [];
    casts // 属性类型转换 protected $casts = [];(
        integer real float double string boolean
        object array collection date datetime timestamp
    )
    hidden // 隐藏字段 protected $hidden = [];
    connection // 数据库联接名,对应config/database.php下的connections(切换数据库用的)
    table // 表名,默认为该模型的复数形式
```


BelongsTo or BelongsToMany

// 相关函数请看Illuminate\Database\Eloquent\Relations\BelongsToMany

// 相关函数请看Illuminate\Database\Eloquent\Relations\BelongsTo

```
class Goods extends Model
{
  public function projects()
  {
    // as 表示中间表的名称默认是pivot, using表示中间表, wherePivot查询中间表
    return $this->belongsToMany(Project::class, 'goods_to_project', 'goods_id', 'project_id')->as('xxxx');
    // 或者
    return $this->belongsToMany(Project::class， null, 'goods_id', 'project_id')->using(GoodsToProject::class);
    // 或者
    return $this->belongsToMany(Project::class, GoodsToProject::getTable(), 'goods_id', 'project_id');
  }
}
$goods = Goods:find(1);
$goods->sync([1, 2, 3, 4, 5]);
$goods->sync([
  1 => [
    'created_at' => new \DateTime(),
    'updated_at' => new \DateTime(),
  ],
]);
$goods->attach([1, 2, 3, 4]);
$goods->attach([
  1 => [
    'created_at' => new \DateTime(),
    'updated_at' => new \DateTime(),
  ],
]);
$goods->detach([1, 2, 3, 4]);
```

// 尝试添加id或者对象，建立关联
attach()
detach()
sync()

// 建立两个模型之间的 belongsTo 关联
associate()
// 取消两个模型之间的 belongsTo 关联
disassociate()
```
$user = User::find(1);
$order = new Order();
$order->user()->associate($user); => $order->user_id = $user->id;
$order->user()->disassociate($user); => $order->user_id = 0;
```

hasOneThrough or hasManyThrough 

Illuminate\Database\Eloquent\Relations\HasManyThrough

Illuminate\Database\Eloquent\Relations\HasOneThrough

```
class Manager
{
  public function managerRoleHospital()
  {
      return $this->hasManyThrough('App\Model\Common\ManagerRoleHospital', 'App\Model\Common\ManagerToRole','manager_id', 'manager_to_role_id', 'id', 'id');
  }
}
Manager::with([
  'managerRoleHospital' => function ($query) {
    // 当前表
    $table = $query->getModel()->getTable() . '.';
    $query->select([
      $table . 'id', 
      $table . 'hospital_id', 
      $table. 'manager_to_role_id',
    ]);
  }
])->get();
```

### 命令行
```
// 创建模型
php artisan make:model Company -mcr
-m 创建迁移文件
-c 创建控制器文件
-r 为控制器添加资源操作方法
```

### 目录结构

```
app // 应用程序核心
  console // 自定义命令及定时任务
  events // 事件类
  exceptions // 异常处理
  http // 请求逻辑
    controllers // 控制器
    middleware // 中间件
    requests // 请求层
  model // 模型
  providers // 服务提供器
  repository // 数据仓库类
  services // 第三方服务
  support // 帮助类, 帮助函数
  jobs // 队列任务
  listeners // 监听处理类
  Traits // 
bootstrap // 引导目录
config // 配置
database // 数据迁移, 数据填充
public // 公共目录
resources // 视图, 及其他未编译文件
routes // 路由
sotrage // 文件缓存, 回话, 日志及文件存储
```
