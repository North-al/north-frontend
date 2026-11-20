# Vue Components

Vue 3 组件设计与单文件组件组织规范。

## 1. Component Responsibilities

组件负责独立的 UI 结构，Hook 负责可复用的业务逻辑。

组件划分遵循实际职责：

- 多个页面存在完全相同的业务 UI 时，可以抽取公共组件。
- 页面内部存在复杂且独立的 UI 区域时，即使只使用一次，也可以拆分。
- 多个页面逻辑相似但 UI 差异明显时，优先考虑 Hook。
- 多个页面结构高度相似时，再考虑抽取组件。
- 只有几行代码的展示组件一般不抽取。
- 不提前创建仅仅为了未来可能复用的组件，除非用户明确要求。

全局公共组件放在 `src/components`，页面专属组件放在对应页面的 `components`。

## 2. Component Complexity

单文件组件过大或职责混乱时，应当主动拆分。

拆分依据：

- 是否具有独立的 UI 职责。
- 是否具有独立的业务逻辑。
- 是否存在明确的状态和交互边界。
- 是否影响当前组件的可读性和维护性。

不要为了减少文件数量，将大量无关逻辑塞进一个组件。

也不要为了拆分而拆分，避免产生大量没有实际意义的小组件。

## 3. Script Setup

默认使用：

```vue
<script setup lang="ts">
</script>

<template>
</template>
```

优先使用 `<script setup>`。

不使用传统 `setup() {}` 或 Options API 作为默认开发方式。

## 4. TSX

项目已经配置 TSX 时，可以根据实际场景使用：

```vue
<script setup lang="tsx">
</script>
```

适合以下情况：

- 后端返回的数据需要在前端加工后动态生成 DOM。
- 不同数据状态对应明显不同的 DOM 结构。
- 模板中堆积大量 `v-if`、`v-else-if`，导致结构难以阅读。
- 使用函数式渲染能够明显提高表达能力和可维护性。

普通模板可以清晰表达的内容，继续使用 Vue Template。

不要为了使用 TSX 而将普通模板改写为 TSX，也不要将整个页面无必要地改成函数式渲染。

## 5. Props and Emits

Props 和 Emits 应具有明确的类型定义。

能够使用 `defineModel` 的场景，优先使用：

```ts
const model = defineModel<string>()
```

避免无意义地重复编写 `modelValue` 和 `update:modelValue`。

但不要求所有组件都使用 `defineModel`，根据实际通信需求决定。

## 6. Props and Slots

Props 与 Slot 根据自定义需求的复杂程度选择。

简单、有限的配置项可以使用 Props，例如 Tag 颜色。

当一个 UI 元素需要暴露多个高度关联的自定义能力时，优先考虑 Slot。

例如，不要为了自定义一个图标，机械地增加 `iconName`、`iconColor`、`iconSize` 等多个 Props。

可以直接开放插槽：

```vue
<template>
  <div class="flex items-center gap-2">
    <slot name="icon" />
    <span>{{ title }}</span>
  </div>
</template>
```

由调用方决定图标类型、颜色和大小。

## 7. defineExpose

接受通过 `defineExpose` 暴露必要的方法。

例如：

```ts
defineExpose({
  open,
  close,
  submit
})
```

适用于弹窗由父组件主动控制、表单需要暴露提交或校验方法等场景。

不要求所有组件都暴露方法。

不要将本来适合 Props / Emits 的数据通信强行改成 `ref` 方法调用。

## 8. Component Communication

根据场景选择通信方式，不固定一种方案。

| 场景 | 优先考虑 |
|---|---|
| 明确的父子数据传递 | Props / Emits |
| 双向绑定 | defineModel |
| 跨层级依赖传递 | provide / inject |
| 跨组件、跨页面共享状态 | Pinia |
| 父组件主动控制子组件 | defineExpose |

不为了避免 Props 就滥用 Store，也不为了避免 Store 进行多层无意义的数据传递。

## 9. Template Logic

模板中可以直接保留简单表达式。

复杂判断、数据加工和业务逻辑应放在 `computed` 或函数中。

遵循：

- 简单表达式直接写在模板中。
- 复杂判断放在合适的计算逻辑中。
- 不为几行简单逻辑创建额外函数。
- 不为了让模板看起来干净而过度抽取。
- 不在模板中堆积复杂业务判断。

不要机械地将所有模板表达式都改成 `computed`。

## 10. Code Organization

组件内部代码应保持清晰、易读。

函数优先使用箭头函数。

命名清晰，不使用过度简写。

必要的注释应当保留，特别是复杂业务规则、设计意图、非直观实现和边界条件。

避免为了形式统一创建大量额外文件、类型文件或辅助函数。

## 11. Existing Components

开发前检查项目已有组件和组件库。

优先复用现有组件。

需要扩展组件能力时，先判断是否适合通过 Props、Slots 或二次封装实现。

如果二次封装的复杂程度超过自行实现，就直接实现，不为了复用而增加复杂度。