# Project Conventions

项目结构和工程组织规范。所有约定均为新项目或没有既定规范时的默认参考，已有项目优先遵循现有设计。

## 1. Project Structure

当项目没有明确约定时，采用以下习惯：

- 页面目录使用 `pages`，不默认使用 `views`。
- 页面目录使用小驼峰，例如 `pages/userCenter`。
- 组件目录使用大驼峰，例如 `components/UserCard`。
- 页面和组件的主文件统一使用 `index.vue`。
- 全局公共组件放在 `src/components`。
- 页面专属组件放在对应页面的 `components`。
- Vue Hooks 统一放在 `hooks`，不使用 `composables` 作为默认目录名称。
- 全局 Hooks 和页面 Hooks 分开管理。
- 页面 Hooks 放在对应页面的 `hooks` 目录。
- API 按业务模块组织。

参考结构：

```text
src/
├── api/
├── components/
├── hooks/
├── pages/
│   └── userCenter/
│       ├── components/
│       ├── hooks/
│       └── index.vue
├── router/
├── stores/
├── setup/
└── main.ts
```

该结构仅作为参考，不得强制改造已有项目。

## 2. Naming

| 类型 | 命名方式 | 示例 |
|---|---|---|
| 页面目录 | 小驼峰 | `userCenter` |
| 组件目录 | 大驼峰 | `UserCard` |
| 页面主文件 | `index.vue` | `pages/userCenter/index.vue` |
| 组件主文件 | `index.vue` | `components/UserCard/index.vue` |
| Hook | `use` 前缀 | `useUser.ts` |
| 初始化函数 | `setup` 前缀 | `setupRouter` |
| API 请求函数 | `fetch` 前缀 | `fetchUserInfo` |

已有项目的命名体系优先，不为了统一命名而大规模重命名。

## 3. Initialization

初始化逻辑采用集中入口调用，各模块提供 `setupXxx` 函数。

例如：

```ts
setupStyles()
setupStore()
setupRouter(app)
setupPlugins(app)
```

初始化函数负责各自模块的配置，不将所有初始化逻辑堆积在 `main.ts` 中。

目录可以参考：

```text
src/setup/
├── styles.ts
├── router.ts
├── store.ts
└── plugins.ts
```

这是一种组织建议，不是必须采用的固定目录。

## 4. Auto Import

Vue 项目应配置：

- Vue API 自动导入。
- 项目组件自动导入。

具体使用何种插件、如何配置，遵循项目现有构建工具和依赖体系。

不要在已有自动导入能力的项目中重复引入另一套实现。

## 5. Styling Solution

样式方案完全遵循项目现有选择。

如果项目使用 UnoCSS 或 Tailwind CSS，按照对应方案编写样式。

不要擅自将现有 CSS、SCSS、UnoCSS 或 Tailwind 项目切换到其他方案。

新项目没有既定方案时，根据项目需求和现有技术栈选择合适方案。

## 6. Dependency Management

引入依赖前，先检查 `package.json` 和现有代码。

遵循：

- 优先使用已有依赖。
- 优先使用项目已有封装。
- 不重复引入功能相同的依赖。
- 不为了个人偏好更换项目依赖。
- 不为了简单需求引入完整工具库。

如果确实需要新增依赖，必须向用户说明：

1. 为什么现有依赖无法满足需求。
2. 新依赖解决什么问题。
3. 是否存在现有依赖或原生 API 的替代方案。

未经用户明确同意，不擅自进行大规模依赖替换或技术栈迁移。

## 7. Project Integration

开发时始终以项目现状为基础。

个人目录习惯、命名习惯和初始化习惯，只在项目没有明确约定时作为默认选择。

**不要为了让项目符合个人习惯而破坏项目原有结构。** 