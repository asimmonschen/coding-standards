# TypeScript 场景示例

## 外部 JSON：类型断言不能验数据（TS-TYPE-02/04）

反例：`const item = JSON.parse(text) as Item`。以下完整片段只接受非空 ID 与非负安全整数数量；生产接口还需在读取文本前限制请求体大小。

```typescript
interface Item {
  id: string;
  count: number;
}

function parseItem(text: string): Item {
  const value: unknown = JSON.parse(text);
  if (typeof value !== 'object' || value === null || Array.isArray(value)) {
    throw new TypeError('item must be an object');
  }
  const fields = value as Record<string, unknown>;
  if (typeof fields.id !== 'string' || fields.id.trim().length === 0) {
    throw new TypeError('id must be non-empty');
  }
  if (typeof fields.count !== 'number' ||
      !Number.isSafeInteger(fields.count) || fields.count < 0) {
    throw new TypeError('count must be a non-negative safe integer');
  }
  return { id: fields.id, count: fields.count };
}
```

这里的断言只把已确认的普通对象当成未知字段容器，业务字段仍逐一校验。若项目已有 schema 校验器，直接复用，不重复手写解析框架。

## 空值默认（TS-TYPE-06）

反例：`const volume = input.volume || 50` 会把合法的零音量改为 50。

```typescript
function volumeOrDefault(volume: number | null | undefined): number {
  return volume ?? 50;
}
```

默认值只处理约定的空值；输入范围仍由调用边界验证。

## 顺序任务必须等待（TS-ASYNC-01/02）

反例：`ids.forEach(async id => await save(id))` 的外层不会等待完成。

```typescript
async function saveInOrder(
  ids: readonly string[],
  save: (id: string) => Promise<void>,
): Promise<void> {
  for (const id of ids) {
    await save(id);
  }
}
```

此例在首个失败时停止，但此前成功的写入不会回滚；需要全有或全无时应使用存储事务或既有补偿方案。不得仅将它换成无界 `Promise.all`。
