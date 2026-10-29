# Python 场景示例

## 避免隐式共享默认值（PY-TYPE-04）

以下完整片段可在 Python 3.8+ 使用。反例 `def add(item, items=[]): ...` 会在多次调用之间复用同一列表。

```python
from dataclasses import dataclass, field
from typing import List, Optional


def add(item: str, items: Optional[List[str]] = None) -> List[str]:
    result = [] if items is None else list(items)
    result.append(item)
    return result


@dataclass
class Batch:
    items: List[str] = field(default_factory=list)
```

此例有意返回新列表，不修改调用方输入。若既有 API 约定原地修改，则保持该契约；修默认参数不能顺带改变对传入列表的行为。

## 注解不执行验证（PY-TYPE-02/05）

```python
def parse_count(value: object) -> int:
    if type(value) is not int:
        raise TypeError('count must be an integer')
    if not 0 <= value <= 100000:
        raise ValueError('count is out of range')
    return value
```

这里显式拒绝 True/False；只写 `isinstance(value, int)` 会接受它们。其他领域可能允许 int 子类，是否使用严格类型检查需按边界契约决定。

## 可观察的异常转换（PY-ERR-01/02）

```python
import json


class ConfigError(ValueError):
    pass


def parse_object(text: str) -> dict:
    try:
        value = json.loads(text)
    except json.JSONDecodeError as exc:
        raise ConfigError('configuration is not valid JSON') from exc
    if not isinstance(value, dict):
        raise ConfigError('configuration must be an object')
    return value
```

返回值只保证对象形状，还没有验证业务字段；先限制输入文本大小。禁止把此例改成 `except Exception: return {}`，那会把损坏配置伪装成成功的空配置。

## 取消与线程（PY-ASYNC-02/03）

取消一个等待线程执行器的 asyncio task，并不保证线程中的阻塞调用停止。先确认实际网络库的连接/读取超时；宿主关闭时仍需处理尚未结束的工作，不能仅对包装 Future 调用 cancel 就声称资源已释放。
