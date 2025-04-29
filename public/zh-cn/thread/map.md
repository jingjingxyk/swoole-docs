# 安全并发容器 Map

创建一个并发的 `Map` 结构，可作为线程参数传递给子线程。读写时在其他线程是可见的。

## 特性
- `Map`、`ArrayList`、`Queue` 会自动分配内存，不需要像 `Table` 那样固定分配。

- 底层会自动加锁，是线程安全的。

- 可传递的变量类型参考 [数据类型](thread/transfer.md)。

- 不支持迭代器，可使用 `keys()`、`values()`、`toArray()` 来代替。

- 必须在线程创建前将 `Map`、`ArrayList`、`Queue` 对象作为线程参数传递给子线程。

- `Thread\Map` 实现了 `ArrayAccess` 和 `Countable` 接口，可以直接作为数组操作。

## 示例
```php
use Swoole\Thread;
use Swoole\Thread\Map;

$args = Thread::getArguments();
if (empty($args)) {
    $map = new Map;
    $thread = new Thread(__FILE__, $i, $map);
    sleep(1);
    $map['test'] = uniqid();
    $thread->join();
} else {
    $map = $args[1];
    sleep(2);
    var_dump($map['test']);
}
```

- 增加或修改：`$map[$key] = $value`
- 删除：`unset($map[$key])`
- 读取：`$value = $map[$key]`
- 获取长度：`count($map)`

## 方法

### __construct()
安全并发容器 `Map` 构造函数

```php
Swoole\Thread\Map->__construct(?array $values = null)
```

- `$values` 可选，遍历数组将数组中的值添加到 `Map` 中

### add()
向 `Map` 中写入数据

```php
Swoole\Thread\Map->add(mixed $key, mixed $value) : bool
```
  * **参数**
      * `mixed $key`
          * 功能：需要添加的key。
          * 默认值：无。
          * 其它值：无。
  
      * `mixed $value`
          * 功能：需要添加的的值。
          * 默认值：无。
          * 其它值：无。
  
  * **返回值**
      * 如果 `$key` 已存在，返回 `false`，否则返回 `true` 表示添加成功。

### update()
更新 `Map` 中的数据

```php
Swoole\Thread\Map->update(mixed $key, mixed $value) : bool
```

  * **参数**
      * `mixed $key`
          * 功能：需要修改的key。
          * 默认值：无。
          * 其它值：无。
  
      * `mixed $value`
          * 功能：需要修改的的值。
          * 默认值：无。
          * 其它值：无。
  
  * **返回值**
      * 如果 `$key` 不存在，返回 `false`，否则返回 `true` 表示更新成功

### incr()
使 `Map` 中的数据安全地自增，支持浮点型或整型，若对其他类型进行自增操作，将会自动转为整型，初始化为 `0`，再进行自增操作

```php
Swoole\Thread\Map->incr(mixed $key, mixed $value = 1) : int | float
```
* **参数**
    * `mixed $key`
        * 功能：需要自增的key，如果不存在则自动创建，并初始化为 `0`。
        * 默认值：无。
        * 其它值：无。

    * `mixed $value`
        * 功能：需要自增的值。
        * 默认值：1。
        * 其它值：无。

* **返回值**
    * 返回自增后的值。

### decr()
使 `Map` 中的数据安全地自减，支持浮点型或整型，若对其他类型进行自减操作，将会自动转为整型，初始化为 `0`，再进行自减操作

```php
Swoole\Thread\Map->decr(mixed $key, mixed $value = 1) : int | float
```
* **参数**
    * `mixed $key`
        * 功能：需要自减的key，如果不存在则自动创建，并初始化为 `0`。
        * 默认值：无。
        * 其它值：无。

    * `mixed $value`
        * 功能：需要自减的值。
        * 默认值：1。
        * 其它值：无。

* **返回值**
    * 返回自减后的值。

### count()
获取元素数量

```php
Swoole\Thread\Map()->count(): int
```

  * **返回值**
      * 返回Map中的元素数量。

### keys()
返回所有 `key`

```php
Swoole\Thread\Map()->keys(): array
```

  * **返回值**
    * 返回 `Map` 所有的 `key`

### values()
返回所有 `value`

```php
Swoole\Thread\Map()->values(): array
```

* **返回值**
    * 返回 `Map` 所有的 `value`

### toArray()
将 `Map` 转换为数组

```php
Swoole\Thread\Map()->toArray(): array
```

### clean()
清空所有元素

```php
Swoole\Thread\Map()->clean(): void
```

### sort()
对容器内的元素进行排序。底层会保存`key`与`value`的映射关系，仅对`value`进行排序。与`asort()`的行为是一致的。

```php
Swoole\Thread\Map()->sort(): void
```

