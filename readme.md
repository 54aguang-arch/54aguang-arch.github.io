# MongoDB JSON 转换器

一个纯前端静态页面工具，用于将 MongoDB 扩展 JSON 格式转换为标准 JSON，无需任何后端服务，直接在浏览器中打开即可使用。

## 功能一览

### 核心转换

- **MongoDB 类型自动转换**：支持 `ObjectId`、`ISODate`、`NumberLong`、`NumberInt`、`NumberDecimal`、`UUID`、`BinData`、`DBRef`、`Timestamp`、`MinKey`、`MaxKey`、`undefined` 等 MongoDB 扩展类型到标准 JSON 的转换
- **无引号键名兼容**：自动为 `{key: value}` 形式的无引号键名添加双引号，使其成为合法 JSON
- **多文档批量处理**：支持同时粘贴多个文档，自动识别并拆分，输出为 JSON 数组
- **嵌套 JSON 解析**：可选开启递归解析嵌套在字符串中的 JSON 内容（如字段值本身是 JSON 字符串）
- **自动转换**：输入内容后 600ms 自动触发转换，也可手动点击"立即转换"

### 输出控制

- **Monaco 编辑器**：输出区使用 Monaco Editor，支持语法高亮、代码折叠、搜索替换
- **字段过滤**：打开字段过滤面板，勾选需要保留的字段后重新生成输出，支持全选、反选、搜索
- **字体大小调节**：输出区字体大小可在 9px–24px 间调整，设置自动持久化到 localStorage
- **换行切换**：一键开启/关闭输出区自动换行
- **编辑模式**：可切换输出区为可编辑状态，方便手动微调
- **全屏查看**：输出内容可铺满全屏，独立 Monaco 编辑器窗口
- **复制 / 下载**：一键复制 JSON 到剪贴板，或下载为 `.json` 文件

### 辅助工具

- **时间转换**：毫秒时间戳 ↔ 日期时间双向转换，支持"当前时间"快捷填入
- **编解码工具**：Base64、URL 编码、Unicode 转义、HTML 实体、Hex、JWT 解析 六种编解码方式，支持编码/解码双向操作和"输出填回输入"的链式操作
- **历史记录**：每次转换自动保存到 localStorage（最多 20 条），可随时还原、删除、清空

### 界面交互

- **明暗主题切换**：深色/浅色两套完整配色方案，Monaco 编辑器主题同步切换
- **可拖拽分割线**：左右面板宽度可自由拖拽调整
- **右键菜单增强**：输出区右键支持"时间戳转时间"和"复制原始值（去引号）"两个快捷操作
- **快捷键**：输入区 `Ctrl+Enter` 立即触发转换
- **状态通知**：转换成功/失败/警告均有顶部浮动提示

## 支持的 MongoDB 类型转换规则

| MongoDB 类型 | 输入示例 | 转换结果 |
|---|---|---|
| ObjectId | `ObjectId("507f1f77bcf86cd799439011")` | `"507f1f77bcf86cd799439011"` |
| ISODate | `ISODate("2024-01-01T00:00:00Z")` | `"2024-01-01T00:00:00Z"` |
| NumberLong | `NumberLong(100)` 或 `NumberLong("100")` | `100` |
| NumberInt | `NumberInt(50)` | `50` |
| NumberDecimal | `NumberDecimal("3.14")` | `"3.14"` |
| UUID | `UUID("abc123")` | `"abc123"` |
| BinData | `BinData(0, "AQID")` | `"[BinData:AQID]"` |
| DBRef | `DBRef("coll", ObjectId("id"))` | `{"$ref":"coll","$id":"id"}` |
| Timestamp | `Timestamp(1609459200, 1)` | `1609459200` |
| MinKey | `MinKey` | `"MinKey"` |
| MaxKey | `MaxKey` | `"MaxKey"` |
| undefined | `undefined` | `null` |
| 无引号键名 | `{name: "test"}` | `{"name": "test"}` |

## 使用方式

直接在浏览器中打开 `mongoFormat.html` 文件即可使用，无需安装任何依赖。

> Monaco 编辑器和字体资源通过 CDN 加载（npmmirror 和 Google Fonts），需网络连接。

## 技术依赖

- **Monaco Editor**（0.55.1）— 来自 npmmirror CDN，用于输出区和全屏模式的代码编辑与高亮
- **Google Fonts** — JetBrains Mono（代码字体）、Outfit（标题字体）、Noto Sans SC（中文 UI 字体）
- 纯 HTML + CSS + JavaScript，无构建步骤、无框架依赖、无后端