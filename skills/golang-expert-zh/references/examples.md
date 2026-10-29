# Go 场景示例

## typed nil 并不是 nil 接口（GO-TYPE-03）

```go
package example

import (
    "context"
    "fmt"
)

type LoadError struct{ Name string }

func (e *LoadError) Error() string { return "load failed: " + e.Name }

// 错误示范：返回的 error 包含动态类型 *LoadError，因此不等于 nil。
func badSuccess() error {
    var err *LoadError
    return err
}

func goodSuccess() error { return nil }

// Fetcher 的实现必须遵守 ctx，并在取消时及时结束。
type Fetcher interface {
    Fetch(context.Context, string) ([]byte, error)
}

func Load(ctx context.Context, fetcher Fetcher, key string) ([]byte, error) {
    data, err := fetcher.Fetch(ctx, key)
    if err != nil {
        return nil, fmt.Errorf("load key %q: %w", key, err)
    }
    return data, nil
}
```

示例 key 视为非敏感标识，真实项目不能把密码、凭据或隐私标识直接写进错误。`%w` 在此有意把底层错误身份暴露给调用方。

## 线程安全不等于只有一个原子变量（GO-CONC-02）

当余额和版本号共同表示一笔状态时，分别原子更新仍可能被读到不一致组合。默认用同一把锁保护读写，或发布一份不可变快照；只有证明内存序与不变量成立才改用无锁算法。

## 版本适配（GO-CONC-04/05）

旧项目不能直接复制新标准库的 `WaitGroup.Go`。Go 1.22 的循环变量语义受模块语言版本影响，也不能解决所有闭包共享变量问题。根据目标版本选择明确的值传递；先读真实构建配置，不机械给每个循环加 `v := v`。
