# Monokai Sky

一款基于 Monokai 风格深度定制的 VS Code 暗色主题，在保留 Monokai 经典配色的基础上优化了语法高亮层次，带来更舒适的编码体验。

## 特性

- 基于 Monokai 的暗色主题（`vs-dark`）
- 优化的语法高亮配色，层次分明
- 兼容 VS Code `^1.100.0`

## 环境准备

在开始之前，请确保已安装以下工具：

- **Node.js**（建议 LTS 版本，自带 npm）：[https://nodejs.org](https://nodejs.org)
- **Visual Studio Code**：[https://code.visualstudio.com](https://code.visualstudio.com)
- **Git**（可选，用于版本管理）：[https://git-scm.com](https://git-scm.com)

验证安装是否成功：

```bash
node -v
npm -v
code -v
```

## 从零创建主题扩展（脚手架）

如果需要从零开始创建一个类似的本项目，可按以下步骤操作。

### 1. 安装 Yeoman 与官方脚手架生成器

```bash
npm install -g yo generator-code
```

### 2. 生成扩展项目

在目标目录执行：

```bash
yo code
```

生成器会依次询问以下问题，主题类扩展可参考如下选项：

| 提示 | 选择/输入 |
| --- | --- |
| What type of extension do you want to create? | `New Color Theme`（新颜色主题） |
| Do you want to import or convert an existing TextMate theme? | `No, start fresh`（从零开始） |
| What's the name of your extension? | `Monokai Sky` |
| What's the identifier of your extension? | `monokai-sky`（默认即可） |
| What's the description of your extension? | 主题描述，如 `A customized Monokai-inspired dark theme.` |
| Select a base theme | `Dark`（暗色基底） |
| Select a theme style | 可选 `Monokai` 风格 |
| Initialize a git repository? | `Yes`（推荐） |

生成完成后，目录结构如下：

```
monokai-sky/
├── .vscode/
│   └── launch.json          # 调试配置（F5 启动扩展开发宿主）
├── themes/
│   └── Monokai Sky-color-theme.json   # 主题配色定义文件（核心）
├── .vscodeignore            # 打包时需要排除的文件列表
├── CHANGELOG.md             # 版本变更记录
├── LICENSE                  # 许可证
├── package.json             # 扩展清单（名称、版本、publisher 等）
├── README.md                # 扩展说明文档
└── vsc-extension-quickstart.md
```

### 3. 自定义主题配色

主题的核心是 [themes/Monokai Sky-color-theme.json](themes/Monokai%20Sky-color-theme.json)，主要包含两部分：

- **`colors`**：VS Code 界面配色（编辑器背景、活动栏、状态栏等）
- **`tokenColors`**：语法高亮规则（关键字、字符串、函数、注释等）

修改配色后保存文件即可，无需手动刷新。

### 4. 本地调试与预览

1. 在 VS Code 中打开项目根目录
2. 按 `F5` 启动调试，会弹出一个加载了当前主题的 **扩展开发宿主** 窗口
3. 在宿主窗口中按 `Ctrl+K Ctrl+T` 打开主题选择器，选择 `Monokai Sky` 即可预览效果

## 使用扩展

### 方式一：从 VS Code 市场安装（发布后）

1. 打开 VS Code
2. 按 `Ctrl+Shift+X` 打开扩展面板
3. 搜索 `Monokai Sky` 并点击安装
4. 按 `Ctrl+K Ctrl+T` 选择主题 `Monokai Sky`

### 方式二：本地安装 VSIX 文件

如果已有打包好的 `.vsix` 文件（如 `monokai-sky-0.0.1.vsix`）：

- **图形界面**：扩展面板右上角 `···` 菜单 → `从 VSIX 安装...`，选择文件后重启 VS Code
- **命令行**：

```bash
code --install-extension monokai-sky-0.0.1.vsix
```

## 使用 vsce 打包

[vsce](https://github.com/microsoft/vscode-vsce) 是 VS Code 官方的扩展打包与发布工具。

### 1. 安装 vsce

```bash
npm install -g @vscode/vsce
```

### 2. 打包为 VSIX

在项目根目录执行：

```bash
vsce package
```

成功后会在当前目录生成 `monokai-sky-<version>.vsix` 文件。

### 3. 打包常见问题

**缺少 repository 字段**

```
ERROR: Make sure to edit the README.md file before you package or publish your extension
```

`package.json` 中必须包含有效的 `repository` 字段：

```json
"repository": {
  "type": "git",
  "url": "https://github.com/loskyertt/monokai-sky.git"
}
```

**缺少 LICENSE**

vsce 要求扩展必须包含许可证文件。可在项目根目录放置 `LICENSE` 文件，或在 `package.json` 中声明：

```json
"license": "MIT"
```

**README.md 为模板默认内容**

打包前会校验 README.md，请确保已替换为真实的项目介绍（即本文档），否则会打包失败。

**publisher 未设置**

`package.json` 中的 `publisher` 字段必须填写你的市场发布者 ID（本项目中为 `loskyertt`）。若仅本地使用不发布，也可在打包时忽略该警告。

### 4. 更新版本号

发布新版本前，建议先更新版本号：

```bash
# 补丁版本 0.0.1 -> 0.0.2
vsce patch

# 次版本 0.0.1 -> 0.1.0
vsce minor

# 主版本 0.0.1 -> 1.0.0
vsce major
```

也可以直接修改 `package.json` 中的 `version` 字段后重新执行 `vsce package`。

## 发布到 VS Code 市场（可选）

若希望将主题发布到 [VS Code Marketplace](https://marketplace.visualstudio.com)，步骤如下：

1. 前往 [Azure DevOps](https://dev.azure.com) 创建组织，并生成一个 **Personal Access Token**（PAT）， scope 勾选 **Marketplace → Manage**
2. 创建发布者并记住其 ID，填入 `package.json` 的 `publisher` 字段
3. 登录发布者：

   ```bash
   vsce login loskyertt
   ```

   按提示粘贴 PAT
4. 发布扩展：

   ```bash
   vsce publish
   ```

   也可指定版本发布：`vsce publish 0.0.2`

发布成功后，几分钟内即可在市场上搜索到该主题。

## 项目结构说明

```
.
├── themes/                      # 主题配色定义
├── .vscodeignore                # 打包排除规则（排除 .vscode、.git 等）
├── package.json                 # 扩展清单文件
├── LICENSE                      # MIT 许可证
└── CHANGELOG.md                 # 版本日志
```

## 相关资源

- [VS Code 扩展 API 文档](https://code.visualstudio.com/api)
- [颜色主题官方指南](https://code.visualstudio.com/api/references/theme-color)
- [vsce 工具文档](https://github.com/microsoft/vscode-vsce)
- [Yeoman generator-code](https://github.com/microsoft/vscode-generator-code)

## 许可证

[MIT](LICENSE)
