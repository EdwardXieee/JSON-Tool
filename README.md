# JSON Tool

一个轻量的离线 JSON 工具，纯前端实现，无需安装，打开即用。

## 功能

| 功能 | 说明 |
|------|------|
| JSON 对比 | 高亮增删改，支持数组业务键、忽略路径、宽松类型和缺失值规则 |
| JSON Patch | 从 JSON A 到 JSON B 导出 RFC 6902 Patch |
| 格式化 / 压缩 | 美化缩进或一键压缩 JSON |
| 转义 / 反转义 | 处理转义字符（`\"`, `\n` 等） |
| JSON ↔ YAML | 基于内置 js-yaml 的 JSON 与 YAML 互转 |
| 日志 / NDJSON | 提取日志中的 JSON 片段，显示行号与字符位置，支持选择、分别格式化或合并数组 |
| JSONPath | 执行包含递归、通配符和安全过滤表达式的 JSONPath 查询 |
| JSON Schema | 使用 JSON Schema Draft-07 校验数据并输出结构化错误报告 |

## 使用

直接在浏览器中打开 `json-tool.html` 即可使用，所有功能在本地运行，数据不会上传到任何服务器。

### 常用功能

- **格式化 JSON**：打开“格式化 / 压缩”，粘贴 JSON 后点击“格式化”。
- **比较 JSON**：打开“JSON 对比”，左侧放原始数据，右侧放新数据，然后点击“对比”。
- **从日志提取 JSON**：打开“日志提取”，粘贴日志后选择“从普通日志提取 JSON”。
- **解析 NDJSON / JSONL**：如果文本中每一行都是独立 JSON，选择“解析每行一个 JSON”。

### 高级功能

高级功能在页面中均有用途说明和可运行示例，也可以点击右上角“使用帮助”查看完整指南。

| 功能 | 什么时候使用 | 示例 |
|------|--------------|------|
| 宽松类型 | 接口把数字或布尔值返回成字符串时 | `"1"` 和 `1` 视为相同 |
| 缺失等同 `null` | 接口有时省略空字段时 | `{}` 和 `{"note":null}` 视为相同 |
| 数组业务键 | 两份对象数组顺序不同，需要按唯一编号配对时 | 填写 `id` 或 `meta.orderId` |
| 忽略路径 | 时间戳、流水号等字段每次都会变化时 | `$.timestamp`、`$.items[*].updateTime` |
| JSON Patch | 需要给程序生成从 JSON A 修改到 JSON B 的指令时 | `replace /status` |
| JSONPath | 大 JSON 中只想取出某些值时 | `$..id` 查找所有层级的 `id` |
| JSON Schema | 检查字段是否必填、类型是否正确时 | 检查 `id` 必须是字符串 |

## 项目结构

```
json-tool.html   # 唯一源文件，包含全部 HTML / CSS / JS
```

## 特性

- 支持亮色 / 暗色主题切换
- 纯前端，单文件离线部署（第三方组件均已内联，无 CDN 请求）
- 中文界面

## 数据与隐私

- 所有解析和转换都在浏览器本地完成，不发起网络请求。
- 最近使用内容会保存在当前浏览器的 `localStorage` 中，可从各输入框的“历史”菜单清除。

## 内置组件

- [js-yaml](https://github.com/nodeca/js-yaml) 5.4.1，MIT License。浏览器构建已直接内联到 `json-tool.html`，用于离线 YAML 解析与生成。
- YAML → JSON 使用 YAML 1.2 Core Schema；YAML 中 JSON 无法表示的非有限数值会明确报错，不会静默转换为 `null`。
- [JSONPath Plus](https://github.com/JSONPath-Plus/JSONPath) 10.4.0，MIT License。使用 `eval: "safe"` 执行过滤表达式，不启用原生 `eval` 模式。
- [Ajv](https://github.com/ajv-validator/ajv) 8.20.0 浏览器包，MIT License。用于 JSON Schema Draft-07 校验；Schema 限制为 500,000 字符、100 层和 10,000 节点，并关闭额外 format 正则校验。
