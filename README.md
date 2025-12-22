## Neovim 配置（lazy.nvim + snacks.nvim）

这是一套基于 **Neovim 0.11+** 的个人配置：使用 `lazy.nvim` 管理插件、`snacks.nvim` 提供 dashboard / picker，并用 **Neovim 内置 LSP**（`vim.lsp.enable` + `lsp/*.lua`）按文件类型启动语言服务。

### 亮点

- **插件管理**：`lazy.nvim`（带 `lazy-lock.json` 锁定版本）
- **搜索/选择器**：`snacks.nvim`（`<leader>f*` 系列快捷键）
- **文件管理**：`oil.nvim`（`-` 打开父目录）
- **LSP**：按 `after/ftplugin/*.lua` 启动；server 配置在 `lsp/*.lua`
- **格式化**：`conform.nvim`（保存自动格式化 + 手动格式化）
- **Lint**：`nvim-lint`（JS/TS/Vue/JSON 等）
- **Git**：`gitsigns.nvim`、`diffview.nvim`，并可用 `lazygit`
- **UI**：`catppuccin` 主题、`heirline` 状态栏
- **Markdown**：`render-markdown.nvim`、图片粘贴（`img-clip.nvim`）
- **可选 AI**：`avante.nvim`（需要额外依赖/配置）

### 版本与依赖

- **Neovim**：建议 `>= 0.11`（本配置使用 `vim.lsp.enable`、`vim.uv`、`vim.loader.enable()` 等）
- **必备命令**：
  - `git`（首次启动会自动拉取 `lazy.nvim`）
  - `ripgrep`（`rg`，用于内容检索 picker）
  - `fd`（用于文件查找 picker，推荐安装）
- **推荐/可选**：
  - `lazygit`：用于 `<leader>gg`
  - `node` / `npm`：`prettierd`、`eslint_d`、`oxlint` 等（JS/TS/Vue lint/format）
  - `python3` / `pip`：配合 `mason.nvim` 安装 Python 相关工具与 LSP
  - `stylua`：Lua 格式化
  - `imagemagick`：提供 `magick`，用于 Markdown/TeX/Typst 图片粘贴处理（`img-clip.nvim`）

### 安装

1. 备份你现有的配置（如有）：

```bash
mv ~/.config/nvim ~/.config/nvim.bak.$(date +%F-%H%M%S)
```

2. 将本目录放到 `~/.config/nvim`（例如克隆到该路径，或直接复制整个目录）。
3. 启动 `nvim`，首次启动会自动安装 `lazy.nvim` 并同步插件。

常用维护命令：

- `:Lazy`：打开插件管理
- `:Lazy sync`：安装/更新插件
- `:Mason`：安装/管理 LSP/格式化/调试等工具

### 目录结构

- `init.lua`：入口，加载 `lua/custom/*`
- `lua/custom/lazy.lua`：引导并加载 `lazy.nvim`
- `lua/custom/plugins/`：插件声明（lazy spec）
- `lua/custom/config/`：插件配置（被 `plugins/*` 引用）
- `after/ftplugin/`：按文件类型做设置（包含 `vim.lsp.enable(...)`）
- `lsp/`：内置 LSP server 配置（供 `vim.lsp.enable` / `vim.lsp.start` 使用）
- `snippets/`：片段

### 快捷键（摘选）

> Leader 键是 **Space**（见 `lua/custom/options.lua`）

- **Snacks picker**
  - `<leader>ff`：智能找文件
  - `<leader>fw`：搜索内容（grep）
  - `<leader><leader>`：切换 buffer
  - `grr`：LSP references
  - `<leader>fs`：当前 buffer symbols（无 LSP 时回退 treesitter）
- **文件浏览**
  - `-`：打开父目录（Oil）
- **格式化**
  - `<leader>lf`：格式化当前 buffer
  - `<leader>tf`：切换保存自动格式化（Conform）
- **LSP 显示**
  - `<leader>td`：切换 diagnostics 显示
  - `<leader>th`：切换 inlay hints（支持时）
- **Git**
  - `<leader>gg`：打开 `lazygit`（可选安装）
  - `<leader>co`：Diffview 开关
  - `<leader>ch`：当前文件历史（Diffview）

### LSP（如何启用/改配置）

- **启用方式**：按文件类型在 `after/ftplugin/*.lua` 中调用 `vim.lsp.enable(...)`
  - 例如：
    - `python`：`pylsp` + `ruff`
    - `lua`：`lua_ls`
    - `tex`：`texlab`
    - `typst`：`tinymist`
    - `typescript/javascript`：`vtsls`
    - `vue`：`vtsls` + `vue_ls`
- **Server 配置**：在 `lsp/*.lua` 中（如 `lsp/clangd.lua`, `lsp/pyright.lua`, `lsp/texlab.lua` 等）
- **安装 server**：推荐用 `:Mason` 管理（也可用系统包管理器自行安装）

### 格式化与 Lint

- **Conform**：配置在 `lua/custom/config/conform.lua`
  - Lua：`stylua`
  - Web：`prettierd`
  - Markdown：`prettierd` + `cbfmt`（默认使用 `~/.config/cbfmt.toml`）
- **nvim-lint**：配置在 `lua/custom/plugins/nvim-lint.lua`
  - JS/TS/JSON/Vue：`oxlint` / `eslint_d`（按 filetype 组合执行）

### 平台差异说明

部分插件按平台启用（通过 `custom.utils.is_mac()` 判断）：

- `vimtex`：仅 macOS 启用（Linux 下默认关闭）
- `typst-preview.nvim`：仅 macOS 启用


