# scopes 和 digest 相关学习
首先实现 scope 对象的构建，scope 由函数 Scope 作为构建函数生成。

``` javascript
function Scope() {}
var scope = new Scope();
```

在 scope 的原型链上首先要实现两个方法，分别是 `$watch` 和 `$digest`。`$watch` 的作用是在 scope 上创建一个观察对象，对象包含两个属性，分别是 `watchFn` 和 `listenerFn`。前者的作用是返回一个在 scope 上的属性作为观察对象，在每次 `$digest` 被触发的时候将这个观察对象和上次的值进行比较，如果发生了变化，就触发对应的 `listenerFn`。

为了按顺序保存 watcher 对象，在 Scope 构造函数中设置一个数组作为容器：

```javascript
function Scope() {
  this.$$watchers = [];
}
```

那么 `$watch`方法就是往这个容器里 push 一个个 watcher 即可：

```javascript
Scope.prototype.$watch = function(watchFn, listenerFn, valueEq) {
  this.$$watchers.push({
    var watcher = {
      watchFn: watchFn,
      listenerFn: listenerFn || function() {},
      valueEq: !!valueEq,
      last: initWatchVal
    };
    this.$$watchers.push(watcher);
  });
};
```



