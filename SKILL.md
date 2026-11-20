---
name: north-frontend
description: Personal Vue 3 and TypeScript frontend engineering standards. Use when developing, reviewing, debugging, refactoring, or designing Vue 3 applications. Covers project discovery, architecture, components, hooks, reactivity, API integration, Pinia, routing, UI, styling, and engineering practices.
---

# North Frontend

面向 Vue 3 + TypeScript 项目的个人工程规范，用于指导 AI Agent 理解项目、制定技术方案、编写代码、处理问题和交付结果。

本 Skill 不提供强制性的项目模板，而是让 Agent 在尊重现有项目的基础上，遵循合理的工程设计和编码习惯。

## Core Principles

**融入项目，而不是让项目融入我们。**

所有开发行为遵循以下优先级：

1. 用户当前明确提出的要求。
2. 项目现有架构、依赖、规范与实现。
3. 当前需求的实际业务约束。
4. 本 Skill 定义的个人开发偏好。
5. AI 自身的默认实现习惯。

项目已有的技术栈、目录结构、依赖、组件、工具函数和编码习惯，优先级高于本 Skill 中的个人偏好。

只有用户明确要求、现有设计确实存在问题，或者需求无法通过现有架构合理实现时，才考虑调整项目约定。

始终遵循：

- 优先复用，不重复造轮子。
- 适度抽象，不为了复用而复用。
- 尊重现有架构，不擅自重构。
- 保持代码直观，不制造无意义的复杂度。
- 先解决实际问题，不进行没有依据的优化。
- 类型服务于可读性，不追求复杂的类型体操。

## Project Discovery

开始开发前，先理解当前项目。

根据任务需要检查：

- 项目目录结构和模块组织。
- `package.json`、依赖及其版本。
- 现有组件、Hooks 和工具函数。
- API 请求封装和数据结构。
- 路由、权限和状态管理。
- 样式方案、组件库和主题约定。
- 项目已有编码规范。

不要在没有理解项目的情况下直接生成一套新的架构。

优先使用项目已有依赖和封装。确实需要新增依赖时，必须向用户说明引入原因、用途和必要性。

## Workflow

根据任务规模决定开发方式：

- 简单任务：直接实现，不制造额外流程。
- 复杂任务：先分析需求、设计方案、涉及模块和潜在影响，再开始实施。

修改代码时：

- 不随意重构无关代码。
- 不擅自改变项目架构。
- 不覆盖已有实现。
- 修改前理解相关代码的调用关系。
- 尽可能控制修改范围。

## Reference Guidelines

根据任务类型阅读对应参考文件，不要为了完成简单任务机械加载所有文档。

| 任务类型 | 参考文件 |
|---|---|
| 项目结构、目录、命名、初始化 | `references/project-conventions.md` |
| Vue 组件、SFC、Props、Slots、TSX | `references/vue-components.md` |
| Hooks、响应式、VueUse、生命周期 | `references/vue-reactivity.md` |
| API、Axios、Pinia、异常处理 | `references/api-and-state.md` |
| Vue Router、路由组织、权限 | `references/routing.md` |
| UnoCSS、Tailwind、组件库、UI | `references/ui-and-styling.md` |
| TypeScript、代码质量、性能、调试、交付 | `references/engineering-practices.md` |

如果任务跨越多个领域，阅读相关的多个参考文件。

参考文件中的规范同样遵循 Core Principles：不能为了执行个人偏好而破坏项目现有设计。

## Implementation

实现过程中优先复用项目已有的组件、Hooks、工具函数、依赖和业务模式。

不要：

- 为简单字段映射创建额外转换函数。
- 为几行逻辑创建额外文件。
- 为了复用制造多层抽象。
- 无故增加 DTO 转换层或响应包装层。
- 滥用 computed、watch 或 Store。
- 引入没有实际价值的依赖。
- 进行没有问题依据的性能优化。

遇到不确定的 API、复杂用法或版本差异时，优先查阅对应版本的官方文档。

## Validation

根据本次修改涉及的内容选择验证方式：

- 类型相关修改：根据项目情况执行类型检查。
- 代码规范相关修改：根据项目情况执行 ESLint。
- 构建配置、依赖或工程级修改：根据需要执行构建。

不要求每次修改都执行所有检查。

当前不要求主动编写单元测试，也不主动引入测试框架。

## Delivery

完成任务后简洁反馈：

- 修改内容。
- 涉及文件及其相对路径。
- 执行的验证及结果。
- 尚未解决的问题或潜在风险。

不要输出大段开发总结，不重复复述整个开发过程。

## Final Principle

好的代码不是抽象最多、功能最多、保护最多的代码，而是能够以合理的复杂度清晰解决实际问题，并且容易继续维护的代码。