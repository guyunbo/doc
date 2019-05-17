 # 回调地址不能获取channel

部分回调地址并没有继承common类，导致获取不到channel-id\
index\controller目录下 \
\Notify 已增加fix \
\Openplatform 有channel或者shop_id 但未处理 \
\Server 继承了base类，但没有处理channel \
\Sms 有较多的中间层调用，看是否调用的时候增加channel参数，待定 \
\Wxapp 同样继承了base类

有channel参数的接口要先处理 按照common类里面的赋值到request()里
没有用到 \app\model\shop 里的可直接更改

# command 无法获取channel
command里因为无法获取channel-id，实例化的时候会找不到数据库，所以采用的是读取所有 ‘dms_shop’ 开头的库，强制指定Db的连接，原本的 Db 替换成了 $this->db
``` php
$sql = "SELECT SCHEMA_NAME FROM information_schema.SCHEMATA WHERE SCHEMA_NAME like 'dms_shop%'";
$re = Db::connect('main')->query($sql);
$config = Db::getConfig('shop');
foreach ($re as $v) {
    $config['database'] = $v['SCHEMA_NAME'];
    $this->db = Db::connect($config);

    // 原本的处理逻辑
    xxxx
}
```
ReleaseXXX的command命令目前未作更改

# 事务
开启的事务必须使用同一个连接，事务开启后的所有操作必须在同一个库里，跨库操作的话就不要使用事务

# 数据同步
dms和mp数据库的数据应该保持同步，用触发器做个双向同步