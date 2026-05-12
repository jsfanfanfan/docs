# Mintlify `docs.json` 全局设置说明书

本文依据仓库内 [`essentials/settings.mdx`](./settings.mdx)（英文原文标题为 *Global Settings*）整理，面向需要在 **Mintlify** 文档站中配置 `docs.json` 的维护者。字段含义、类型与默认值以该页为准；若上游 Mintlify 更新配置项，请以 [Mintlify 官方文档](https://mintlify.com/docs) 为准并同步修订本说明书。

---

## 1. 文档与文件角色

| 项目 | 说明 |
|------|------|
| **配置文件** | 每个 Mintlify 站点根目录需要一份 **`docs.json`**，用于控制站点名称、导航、外观、API、页脚与反馈等核心行为。 |
| **本说明书范围** | 逐项说明 `docs.json` 中可能出现的**属性（properties）**；不涉及具体 MDX 页面写法。 |

---

## 2. 必填项

### 2.1 `name`（string，必填）

- **含义**：项目名称，用作全局标题等展示。
- **示例**：`"mintlify"`。

### 2.2 `navigation`（`Navigation[]`，必填）

- **含义**：导航结构，由若干「分组」组成，每组包含组名与页面列表。
- **子字段**：
  - **`group`**（string）：分组在侧栏等处显示的名称。示例：`"Settings"`。
  - **`pages`**（string[]）：作为页面使用的 Markdown/MDX 文件的**相对路径**列表。示例：`["customization", "page"]`。

> 更复杂的 `tabs`、嵌套 `group` 等写法见 [`navigation.mdx`](./navigation.mdx)。本说明书仅复述 `settings` 页对 `navigation` 的字段级定义。

---

## 3. 品牌与视觉

### 3.1 `logo`（string **或** object）

- **string**：单一 Logo 图片路径。
- **object**：分别为明/暗模式指定 Logo，并可配置点击跳转。
  - **`light`**（string）：浅色模式下的 Logo 路径。
  - **`dark`**（string）：深色模式下的 Logo 路径。
  - **`href`**（string，默认 `"/"`）：点击 Logo 后跳转的地址。

### 3.2 `favicon`（string）

- **含义**：站点收藏夹图标路径。

### 3.3 `colors`（`Colors`）

全局主题的 **十六进制颜色** 配置。

| 子字段 | 类型 | 必填 | 含义 |
|--------|------|------|------|
| `primary` | string | 是 | 浅色模式下高亮、章节标题、强调等**主色**。 |
| `light` | string | 否 | 深色模式下上述用途的**主色**（文档原文为 dark mode 下的 primary 用途）。 |
| `dark` | string | 否 | **重要按钮**等使用的主色。 |
| `background` | object | 否 | 明/暗模式下的页面背景色。 |
| `background.light` | string | 在 `background` 存在时必填 | 浅色模式背景色（hex）。 |
| `background.dark` | string | 在 `background` 存在时必填 | 深色模式背景色（hex）。 |

### 3.4 `backgroundImage`（string）

- **含义**：显示在每一页背后的背景图 URL 或路径。
- **参考站点**：原文档举例 [Infisical](https://infisical.com/docs)、[FRPC](https://frpc.io)。

---

## 4. 顶栏链接与行动按钮

### 4.1 `topbarLinks`（`TopbarLink[]`）

- **含义**：顶栏中展示的链接列表。
- **每项包含**：
  - **`name`**（string）：按钮或链接显示名称。示例：`"Contact us"`。
  - **`url`**（string）：点击后打开的地址。示例：`"https://mintlify.com/docs"`。

### 4.2 `topbarCtaButton`（Call to Action）

顶栏主要行动按钮配置。

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `type` | `"link"` 或 `"github"` | `"link"` | `link` 显示普通按钮；`github` 根据 `url` 拉取仓库信息（含 Star 数等）。 |
| `url` | string | — | `link` 时为按钮目标 URL；`github` 时为仓库地址。 |
| `name` | string | — | 按钮内文字；**仅当 `type` 为 `link` 时需要**。 |

---

## 5. 版本、锚点与标签页

### 5.1 `versions`（string[]）

- **含义**：版本名称数组。仅在需要在导航栏用**下拉框**切换多套文档版本时使用。

### 5.2 `anchors`（`Anchor[]`）

侧栏或导航中的「锚点」区块配置（图标、颜色、URL 规则等）。

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `icon` | string | — | [Font Awesome](https://fontawesome.com/search?q=heart) 图标名。示例：`comments`。 |
| `name` | string | — | 锚点标签文案。示例：`Community`。 |
| `url` | string | — | **URL 前缀**：路径以该前缀开头的页面归入此锚点；通常对应你存放页面的一组文件夹名。 |
| `color` | string 或渐变对象 | — | 锚点图标背景色（hex）；也可为 `{ "from": "#...", "to": "#..." }` 渐变。 |
| `version` | string | — | 若需仅在选中某文档版本后才显示该锚点，可填对应版本。 |
| `isDefaultHidden` | boolean | `false` | 为 `true` 时，默认隐藏该锚点，直到用户通过**直达站内某页**进入其下文档。 |
| `iconType` | string | `duotone` | 取值之一：`brands`、`duotone`、`light`、`sharp-solid`、`solid`、`thin`。 |

### 5.3 `topAnchor`（object）

覆盖**最顶层锚点**的默认配置。

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `name` | string | `"Documentation"` | 最顶层锚点名称。 |
| `icon` | string | `"book-open"` | Font Awesome 图标名。 |
| `iconType` | string | `duotone` | 同 `anchors` 中 `iconType` 取值集合。 |

### 5.4 `tabs`（`Tabs[]`）

- **含义**：顶部导航 **Tab** 列表。
- **每项包含**：
  - **`name`**（string）：Tab 标签显示名称。
  - **`url`**（string）：属于该 Tab 的页面 URL **起始前缀**；通常与某文件夹名一致。

---

## 6. API 与 OpenAPI

### 6.1 `api`（`API`）

API 文档与 Playground 相关配置；页面级组件详见原文档内链接 [API Components](/api-playground/demo)（路径以你站点为准）。

| 子字段 | 含义 |
|--------|------|
| **`baseUrl`** | 所有 API 端点的基地址。若为**数组**，则用户可在多个基地址间切换。 |
| **`auth`** | 全局鉴权策略（见下表）。 |
| **`playground`** | Playground 展示方式（见下表）。 |
| **`maintainOrder`** | 为 `true` 时，OpenAPI 页面上键顺序与 OpenAPI 文件一致。原文含 **Warning**：该行为将很快默认开启，届时此字段可能被废弃。 |

**`auth` 展开：**

| 子字段 | 含义 |
|--------|------|
| `method` | `"bearer"`、`"basic"` 或 `"key"`。 |
| `name` | Playground 中鉴权参数名称。若 `method` 为 `basic`，格式应为 `[用户名占位名]:[密码占位名]`。 |
| `inputPrefix` | 鉴权输入框默认前缀；例如 `AuthKey` 会使默认展示以 `AuthKey` 为前缀。 |

**`playground` 展开：**

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `mode` | `"show"` \| `"simple"` \| `"hide"` | `show` | Playground 显示、隐藏或仅展示端点（`simple` 无额外交互）。详见 playground 指南。 |

### 6.2 `openapi`（string **或** string[]）

- **含义**：OpenAPI 文件的 **URL** 或**相对路径**；可为单个字符串或字符串数组（多份规范）。
- **示例**（概念上）：
  - 绝对 URL：`"https://example.com/openapi.json"`
  - 相对路径：`"/openapi.json"`
  - 多个：`["https://example.com/openapi1.json", "/openapi2.json", "/openapi3.json"]`

---

## 7. 页脚与反馈

### 7.1 `footerSocials`（`FooterSocials`）

- **含义**：页脚社交媒体链接；**键**为平台标识，**值为**账号 URL。
- **允许的键**（原文列举）：`website`、`facebook`、`x`、`discord`、`slack`、`github`、`linkedin`、`instagram`、`hacker-news`。
- **示例**：

```json
{
  "x": "https://x.com/mintlify",
  "website": "https://mintlify.com"
}
```

### 7.2 `feedback`（`Feedback`）

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `suggestEdit` | boolean | `false` | 开启后显示「建议编辑」类按钮，引导用户通过 Pull Request 提修改。 |
| `raiseIssue` | boolean | `false` | 开启后允许用户就文档内容发起 issue。 |

---

## 8. 深浅色模式

### 8.1 `modeToggle`（`ModeToggle`）

| 子字段 | 类型 | 默认 | 含义 |
|--------|------|------|------|
| `default` | `"light"` 或 `"dark"` | 未设置则跟随**操作系统** | 新用户首次进入时默认浅色或深色。 |
| `isHidden` | boolean | `false` | 为 `true` 时隐藏明/暗切换开关。与 `default` 组合可**强制仅深色或仅浅色**。 |

**仅深色示例：**

```json
"modeToggle": {
  "default": "dark",
  "isHidden": true
}
```

**仅浅色示例：**

```json
"modeToggle": {
  "default": "light",
  "isHidden": true
}
```

---

## 9. 维护建议（非官方原文，便于落地）

1. **先保证 `name` + `navigation`**，站点即可基本可用。  
2. **改色前先定主色与对比度**，再填 `colors`，避免正文与背景对比不足。  
3. **`openapi` / `api`** 与 API 页面强相关，改 `baseUrl` 或 `auth` 后应在 Playground 里实测请求。  
4. **`maintainOrder`** 若文档提示即将默认开启，新建项目可少依赖该开关。  
5. 将本说明书与 [`navigation.mdx`](./navigation.mdx) 对照阅读，可理清 **`navigation` 与 `tabs` / `anchors` 在侧栏与 URL 上的分工**。

---

## 10. 源文件

- 英文交互式字段说明（含组件渲染）：[`essentials/settings.mdx`](./settings.mdx)

若要让本说明书出现在 Mintlify 导航中，可将内容迁移为带 frontmatter 的 `.mdx`，并在 `docs.json` 的 `pages` 中注册对应路径。
