# Vue Reactivity

Vue 3 Composition API、Hooks、响应式系统和副作用生命周期管理规范。

## 1. Hooks Organization

Vue Hooks 统一使用 `hooks` 目录，不默认使用 `composables`。

区分全局 Hooks 和页面 Hooks：

全局 Hooks：

```text
src/hooks/
├── useTable.ts
└── usePayment.ts
```

页面 Hooks：

```text
src/pages/order/
├── hooks/
│   └── useOrder.ts
└── index.vue
```

全局 Hook 适合跨页面或跨业务模块复用的逻辑。

页面 Hook 适合当前页面或当前业务模块使用的逻辑。

## 2. Hook Responsibilities

Hook 负责可复用的业务逻辑，可以包含同一业务领域内的：

- 请求。
- 状态。
- 计算逻辑。
- 业务流程。
- 事件处理。

优先保持一个 Hook。

只有内部逻辑明显复杂并影响可读性时，才考虑拆分。

不要为了函数数量拆分 Hook，也不要为几行重复代码创建多层 Hook。

组件负责独立 UI 结构，Hook 负责业务逻辑。

## 3. Reactivity API

默认优先考虑 `ref`。

根据数据特征选择响应式 API：

| API | 使用场景 |
|---|---|
| `ref` | 默认响应式数据 |
| `shallowRef` | 大型数据、第三方实例、只需要整体替换的数据 |
| `reactive` | 需要直接通过对象属性访问响应式数据 |
| `markRaw` | 明确不需要 Vue 代理的对象 |
| `computed` | 有明确派生状态需求 |
| `watch` | 有明确副作用监听需求 |

不要将这些 API 当成随意使用的性能开关。

## 4. Watch

`watch` 必须具有明确的监听目的。

使用前确认：

- 是否确实需要监听状态变化。
- 是否可以通过正常的响应式数据流实现。
- 是否会产生重复请求或重复副作用。
- 是否需要限制监听范围。
- 是否需要处理旧副作用的清理。

避免无意义监听和不必要的深度监听。

不要为了同步简单状态而堆积多个 Watcher。

## 5. MaybeRefOrGetter

设计可复用 Hook 时，可以根据参数需求使用 `MaybeRefOrGetter<T>` 和 `toValue()`。

例如：

```ts
import { toValue, watchEffect, type MaybeRefOrGetter } from 'vue'

const useExample = (source: MaybeRefOrGetter<string>) => {
  watchEffect(() => {
    const value = toValue(source)
    // 根据响应式参数执行逻辑
  })
}
```

这使 Hook 可以接收普通值、Ref 或 Getter。

需要依赖追踪时，应在 `watch`、`watchEffect` 或 `computed` 的追踪范围内调用 `toValue()`。

不要在响应式追踪范围外提前解包，导致响应式依赖丢失。

不是所有函数参数都需要设计成 `MaybeRefOrGetter`。仅在确实需要兼容多种响应式输入时使用。

## 6. Scope and Cleanup

编写可复用 Hook、工具函数和副作用逻辑时，应当考虑作用域管理和资源清理。

适当使用：

- `getCurrentScope`
- `onScopeDispose`
- `effectScope`
- `watch` 的清理回调

重点关注：

- 事件监听。
- 定时器。
- MutationObserver / ResizeObserver / IntersectionObserver。
- WebSocket 订阅。
- 第三方实例。
- 其他需要显式释放的资源。

资源的创建、更新和销毁必须具有明确的所有权。

响应式目标发生变化时，需要根据实际情况清理旧资源并建立新资源。

可以参考 VueUse 的 `useEventListener` 设计：支持响应式目标，目标变化时更新监听，并清理旧监听。

但不需要为了使用高级 API 而增加复杂度。

## 7. Resource Ownership

页面卸载不等于取消所有异步任务。

已经发出的请求默认可以继续执行，特别是结果可能被 Store 或其他页面使用时。

只有当任务明确属于当前页面、结果不再需要且业务允许时，才考虑取消。

根据资源归属决定清理方式：

- 页面私有轮询：可以在页面离开时停止。
- 全局 WebSocket：不能因为某个页面卸载就关闭。
- 共享订阅：根据实际订阅关系管理。
- 第三方实例：由实际创建和持有它的模块负责释放。

不要将组件卸载等同于取消所有异步操作。

## 8. VueUse Design Principles

编写可复用 Hook 前，优先检查 VueUse 是否已有成熟实现。

重点学习：

- 参数响应式设计。
- `toValue()` 的使用方式。
- `MaybeRefOrGetter<T>` 类型设计。
- `getCurrentScope()` 与 `onScopeDispose()`。
- 响应式目标变化时的副作用管理。
- 资源清理和监听器重建。
- SSR 环境兼容。
- 函数重载与返回类型设计。

如果项目已经安装 VueUse，可以合理复用。

如果没有安装，不要为了一个普通 Hook 擅自引入整个依赖。可以参考其源码设计思想，只实现当前项目真正需要的部分。

## 9. Third-party Instances and SSR

第三方实例不一定适合被 Vue 深度代理。

根据实际需要使用 `shallowRef` 或 `markRaw`。

涉及浏览器全局对象时，应考虑 SSR 环境下对象不存在的情况。

不要默认所有 Vue 项目都是纯浏览器环境。

## 10. Keep It Simple

响应式设计服务于业务，不服务于 API 数量。

优先保证：

- 数据流清晰。
- 副作用边界明确。
- 资源生命周期明确。
- Hook 容易理解。
- 不引入不必要的抽象。