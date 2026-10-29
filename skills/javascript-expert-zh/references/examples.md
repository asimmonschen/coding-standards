# JavaScript 场景示例

## 严格边界解析（JS-VALUE-03/04）

反例：`parseInt('12items', 10)` 返回 12，不能用来确认输入完全由数字组成。

```javascript
function parseCount(value) {
  if (typeof value !== 'string' || !/^(0|[1-9][0-9]{0,5})$/.test(value)) {
    throw new TypeError('count must be an integer string');
  }
  const count = Number(value);
  if (!Number.isSafeInteger(count) || count > 100000) {
    throw new RangeError('count is out of range');
  }
  return count;
}
```

此例只接受约定范围的十进制整数文本，不接受空白、前导零或科学计数法；是否允许这些形式由业务契约决定，不能无意改变已有解析语义。

## 原地排序与浅拷贝（JS-VALUE-06）

```javascript
function sortedByCount(items) {
  return [...items].sort((left, right) => left.count - right.count);
}
```

调用前已验证 count 是有限数值。此例只避免修改数组顺序，数组元素仍然共享；不得把它当深拷贝后继续修改元素字段。

## 顺序与部分成功（JS-ASYNC-01/02）

```javascript
async function saveInOrder(ids, save) {
  for (const id of ids) {
    await save(id);
  }
}
```

不同于 `ids.forEach(async id => save(id))`，返回的 Promise 会表达完成或失败。但此前成功写入不会回滚；需要原子性时使用既有事务/补偿，而不是假定 await 自动提供事务。

## 模块边界（JS-MOD-01）

同一个 `require` 能否加载某包取决于 Node 版本、exports、构建产物及异步模块特征。遇到加载错误先核实宿主，不把全仓 `require` 替换为 `import` 当作局部修复。
