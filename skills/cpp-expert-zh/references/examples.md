# C++ 场景示例

## Rule of Zero 与可移动资源（CPP-OWN-01/04）

下面是可在 C++11 编译的完整片段。`unique_ptr` 默认析构与移动即可，不手写 delete，也不手写多余的特殊成员。

```cpp
#include <memory>

class Buffer {
 public:
  explicit Buffer(unsigned size)
      : data_(new unsigned char[size]()) {}

 private:
  std::unique_ptr<unsigned char[]> data_;
};
```

该类型的复制被成员禁止、移动由编译器生成。构造函数允许分配异常，适用于允许异常的项目，且外部 size 应先有上限约束。真实缓冲区接口若还维护长度，需一并设计移动后的不变量。

## 条件 noexcept（CPP-ERR-03）

```cpp
#include <type_traits>
#include <utility>

template <class T>
class Holder {
 public:
  explicit Holder(T value) : value_(std::move(value)) {}
  Holder(const Holder&) = delete;
  Holder& operator=(const Holder&) = delete;
  Holder(Holder&&) noexcept(std::is_nothrow_move_constructible<T>::value) = default;
  Holder& operator=(Holder&&)
      noexcept(std::is_nothrow_move_assignable<T>::value) = default;

 private:
  T value_;
};
```

这展示了确需显式表达特殊成员策略时的条件声明。普通包装类型优先让编译器生成，不必照抄模板。反例是对含可能抛出成员的移动操作无条件承诺 `noexcept`。

## 引用捕获与 this（CPP-CONC-04）

危险示意：函数返回后才执行的回调捕获局部字符串引用，或捕获已经析构的 `this`。改法取决于所有权：独立消息按值捕获；对象由任务真正共享拥有时捕获对应 `shared_ptr`；只观察时捕获 `weak_ptr` 并在执行时检查；宿主拥有的任务则在宿主析构前取消并等待。

不要为了修一处回调把整个对象图转成 `shared_ptr`。同步调用且不会逸出的 lambda 可以合法借用局部对象。
