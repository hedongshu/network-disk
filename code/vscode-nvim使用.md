---
date: 2022-01-11 09:46:36
tags:
  - vim
  - vscode
title: vscode-nvim 使用
---

# vscode-nvim 使用

它与 vscode-vim 插件还是有非常多的区别的，vscode-vim 只是一个 vim 模拟器，好处是配置简单方便，不需要掌握过多的 vimrc 相关知识，缺点是运行慢，卡顿，没有原生 vim 操作的丝滑感，此外缺失许多原生 vim 的功能，如宏录制。

而 vscode-nvim 则是将 neovim 与 vscode 真正的连接起来，为你带来如丝般顺滑的编程体验，缺点是配置比较麻烦，需要在系统中安装 neovim，并且所有的配置项会分为 2 部分，这在跨平台同步配置时不太方便。

不管你有没有使用过 vscode-nvim 插件，这篇文章都会给你带来一定的帮助，因为 vscode-nvim 插件的中文资料现在貌似不多，官方文档对于一些新手向的知识描述也不是很全，所以我才打算写了这一篇文章让更多的 vscode 用户走向 vim 的怀抱。

## 下载安装

nvim 是 vim 的一个分支版本，它更强大也更优秀，因此对于现在很多已经将工作环境完全迁徙到终端下的程序员来说，nvim 是一个更好的选择。

大多数 Linux 发行版中都内置安装了 vi 或者 vim，但是 neovim 是肯定没有安装的，所以我们首先要安装 neovim：

```shell
# 1.下载nvim安装包
$ wget https://github.com/neovim/neovim/releases/download/stable/nvim-macos.tar.gz

# 2.解压到指定的目录
$ tar xzf ./nvim-macos.tar.gz -C /usr/local/application/
$ mv /usr/local/application/nvim-osx64/ /usr/local/application/nvim

# 3.将nvim添加到系统环境变量，你可以创建软链接或者直接编辑/etc/profile文件
PATH=/usr/local/application/nvim/bin:$PATH
```

nvim 安装完成之后，打开 vscode 插件商店，搜索 neovim 插件并安装：

![image-20211101230944834](https://images-1302522496.cos.ap-nanjing.myqcloud.com/img/image-20211101230944834.png)

## 基础配置

安装完成之后我们需要对 vscode 进行一些基础设置。

```cpp
1. 仅MAC用户，关闭MAC的重复键
$ defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false

2. 为代码编辑区域设置相对行号以方便vim模式操作，在settings.json中添加上以下配置项
"editor.lineNumbers": "relative",

3. 关闭自动传参建议，使用按键手动触发，在settings.json中添加上以下配置项
"editor.parameterHints.enabled": false,

4. 控制资源管理器中的键盘导航无法自动触发，在settings.json中添加上以下配置项
"workbench.list.automaticKeyboardNavigation": false,

5. 控制编辑器是否可以将光标移动到最后一行以后或者最上一行以上，这对vim模式下的gg、G命令来说更加友好，在settings.json中添加上以下配置项
"editor.scrollBeyondLastLine": false,
```

初始工作做完以后，接下来就需要将 nvim 与 vscode-nvim 插件链接起来，使之互相之间产生关系，你需要根据自己所使用的平台在 settings.json 中加入下面这个配置项，添加完成后需要重启 vscode 实例：

```swift
// Windows
"vscode-neovim.neovimExecutablePaths.win32": "D:\\Application\\Neovim\\bin\\nvim.exe",

// Linux
"vscode-neovim.neovimExecutablePaths.linux": "/usr/local/application/nvim/bin/nvim",

// Mac
"vscode-neovim.neovimExecutablePaths.darwin": "/usr/local/application/nvim/bin/nvim",
```

## 设计哲学

以下是关于 vscode-nvim 如何工作的，摘自官方文档：

- VScode 连接到 neovim 实例
- 打开某个文件时，会在 nvim 中创建一个暂存缓冲区，并使用来自 vscode 的文本内容进行初始化
- NORMAL / VISUAL 模式命令直接发送到 neovim。该扩展程序侦听缓冲区事件并应用来自 neovim 的编辑
- 当进入插入模式时，扩展停止监听击键事件并将打字模式委托给 vscode（这里没有执行 Neovim 通信）
- 从插入模式按下退出键后，扩展程序将从插入模式获得的更改发送到 Neovim

## 差异对比

以下是 vscode-nvim 与原生 nvim 的使用差异对比，摘自官方文档：

- 例如命令模式下的:e、:w、:q、:sp 系列、:tab 系列等命令实际上都不是调用的 nvim 指令完成的，而是调用的 vscode 指令，不要像在 nvim 中一样使用它们，比如 ctrl+r 命令在 nvim 下是 redo 命令，而在 vscode 下则不会有任何效果，你可以在后面通过更改热键来改变它们
- 可视模式不会产生 vscode 选择，因此任何需要选择的 vscode 命令都不起作用。但是可以通过默认热键(f1/ctrl/cmd+shift+p)从可视模式调用 vscode 命令选择器将 vim 选择转换为真正的 vscode 选择。对于某些命令（例如注释和格式设置），此转换也会自动完成。如果您使用一些自定义映射来调用依赖于真实 vscode 选择的 vscode 命令，您可以使用 VSCodeNotifyRange/VSCodeNotifyRangePo（第一个逐行，后一个字符），它将在调用命令之前会将 vim 视觉模式选择转换为 vscode 选择
- 当您键入某些命令时，它们可能会替换另一个命令，例如:write 将替换为:Write.这是正常的。
- 滚动是由 VSCode 完成的。而 `<C-d> / <C-u> / etc`略有不同
- 编辑器自定义（相对行号、滚动条等）由 VSCode 处理
- 点重复(.)略有不同-在更改范围内移动光标不会破坏重复序列。在 Neovim 中，如果您`abc<cursor>`在插入模式下输入，则将光标移至`a<cursor>bc`并 1 在此处输入重复序列将是 1.但是在 vscode 中它会是 a1bc.另一个区别是，当您在插入模式下删除某些文本时，点重复仅从右到左起作用，这意味着在运行点重复时它会将 Del 键视为 BS 键

# 定义热键

## 启动模式

不乏有一些之前使用过 nvim 的用户对 nvim 本身设置了很多选项。

但其实这些选项可能会对 vsocde-nvim 的性能产生影响，为了屏蔽这些影响，vscode-nvim 官方也贴心的编写了 vimscript 来区分你的 nvim 启动模式。

我们只需要将这段 vimscript 放置在~/.config/nvim/init.vim 文件中即可。

- Ps：~/.config/nvim/init.vim 文件是 nvim 的配置文件，如果不存在，那么请创建它

如下所示：

```perl
" 选择启动模式、作为vscode扩展启动、或者作为本体启动
" 时所加载的配置项
if exists('g:vscode')
     " VSCode extension
else
     " ordinary neovim
endif
```

举一个简单的例子，我下面有 3 个 nvim 选项：

- set mouse=a：让 nvim 支持鼠标操作，我希望在 nvim 正常启动时加载
- set nobackup：不创建备份文件，我希望在 nvim 作为 vscode-nvim 插件依赖进行启动时加载
- let mapleader = "\<space>"：设置前导键，我希望无论 nvim 在那种情况下启动都进行加载

这个时候就可以对~/.config/nvim/init.vim 文件进行这样的编辑：

```sql
if exists('g:vscode')
    " 以vsocde-nvim插件作为依赖启动时加载的配置项
    set nobackup
else
    " 以正常模式启动nvim时加载的配置项
    set mouse=a
endif

" 始终都会加载的配置项
let mapleader = "\<space>"
```

另外对于一些插件来说，你也可以单独的指定它们的启动模式，如禁用原生的 vim-easymotion，而启用 vscode-nvim 作者改版的 vim-easymotion，这个内容在下面会介绍到：

```perl
" inside plug#begin:
" use normal easymotion when in vim mode
Plug 'easymotion/vim-easymotion', Cond(!exists('g:vscode'))

" use vscode easymotion when in vscode mode
Plug 'asvetliakov/vim-easymotion', Cond(exists('g:vscode'), { 'as': 'vsc-easymotion' })
```

## 调用 vscode 命令

nvim 除开可以定义自己本身的热键外，还可以通过一些函数间接的调用 vscode 下的命令，这里只例举一些异步调用的函数：

```scss
- 使用可选参数调用vscode命令
VSCodeNotify(command, ...)

- 将line1到line2的可视模式选择变更为vscode的选择，并且会调用vscode命令，如果将leaveSelection设置成1，会在调用命令后删除vscode选择
VSCodeNotifyRange(command, line1, line2, leaveSelection ,...)

- 将line1.pos1到line2.pos2的可视模式选择变更为vscode的选择，并且会调用vscode命令，如果将leaveSelection设置成1，会在调用命令后删除vscode选择
VSCodeNotifyRangePos(command, line1, line2, pos1, pos2, leaveSelection ,...)

- 将可视模式下的选择变更为vscode的选择，并且会调用vscode命令，如果将leaveSelection设置成1，会在调用命令后删除vscode选择
VSCodeNotifyVisual(command, leaveSelection, ...)
```

## 个人热键方案

使用 vim 或者 nvim 插件时，如何配置热键是一个十分棘手的问题，下面是我自用的一些热键定义方案。

我目前使用的是 mac 平台，所以 cmd 按键会比 ctrl 按键更加常用，如果是 windows 或者 linux 平台，则将 cmd 替换为 ctrl 按键即可。

```dos
cmd + g c ： 显示命令面板
cmd + g s ： 打开设置页面
cmd + g k ： 打开热键映射
cmd + g d ： 打开一个目录
cmd + g f ： 打开一个文件
cmd + g h ： 打开最近记录
cmd + g n ： 新建vscode实例
cmd + g q ： 关闭vscode示例

cmd + f n ： 新建文件
cmd + f o ： 打开文件
cmd + f e ： 另存为文件
cmd + f s ： 保存文件
cmd + f w ： 保存所有文件
cmd + f q ： 关闭文件
cmd + f a ： 关闭所有文件

cmd + n [ ： 切换侧边栏显示状态
cmd + n 1 ： 显示文件资源管理器
cmd + n 2 ： 显示TODO Tree
cmd + n 3 ： 显示全局搜索
cmd + n 4 ： 显示debug
cmd + n 5 ： 显示版本控制
cmd + n 6 ： 显示SQL Tools
cmd + n 7 ： 显示Docker
cmd + n 8 ： 显示测试
cmd + n 9 ： 显示插件商店

cmd + p ] ： 切换面板显示状态
cmd + p 1 ： 显示问题
cmd + p 2 ： 显示输出
cmd + p 3 ： 显示终端
cmd + p 4 ： 显示调试控制台
cmd + p 5 ： 显示SQL CONSOLE

cmd + q ：关闭当前选项卡或分屏
cmd + e ：聚焦在第一个选项卡中
cmd + , ：切换到上一个选项卡
cmd + . ：切换到下一个选项卡

cmd + h ： 触发帮助提示
cmd + j ： 触发参数提示
cmd + k ： 触发建议提示
cmd + m ： 触发快速修复
cmd + n ： 移动到下一个建议
cmd + p ： 移动到上一个建议
tab     ： 选择下一个建议
enter   ： 选择当前建议

cmd + = ： 放大字体
cmd + - ： 缩小字体
cmd + alt + l ：格式化代码（个人习惯）

zR ： 展开所有折叠
zM ： 关闭所有折叠
zo ： 展开当下折叠
zc ： 关闭当下折叠
zz ： 切换当下折叠

jj ： 退出INSERT模式
zz ： 切换代码折叠（原生vim的zz不是切换折叠）
gc ： 进行行注释
gC ： 进行块注释


H ：跳转行首、取代^
L ：跳转行尾、取代$

g[ ： 跳转到上一个问题
g] ： 跳转到下一个问题
```

## 我的 keybindings

以下是我的 keybindings.json 文件：

```json
[
  // 按jj退出INSERT模式
  {
    "command": "vscode-neovim.compositeEscape1",
    "key": "j",
    "when": "neovim.mode == insert && editorTextFocus",
    "args": "j"
  },
  // 显示命令面板
  {
    "key": "cmd+g c",
    "command": "workbench.action.showCommands"
  },
  // 打开设置页面
  {
    "key": "cmd+g s",
    "command": "workbench.action.openSettings"
  },
  // 打开热键映射
  {
    "key": "cmd+g k",
    "command": "workbench.action.openGlobalKeybindings"
  },
  // 打开一个目录
  {
    "key": "cmd+g d",
    "command": "workbench.action.files.openFolder"
  },
  // 打开一个文件
  {
    "key": "cmd+g f",
    "command": "workbench.action.files.openFile"
  },
  // 打开最近记录
  {
    "key": "cmd+g h",
    "command": "workbench.action.openRecent"
  },
  // 新建vscode实例
  {
    "key": "cmd+g n",
    "command": "workbench.action.newWindow"
  },
  // 关闭vscode实例
  {
    "key": "cmd+g q",
    "command": "workbench.action.closeWindow"
  },
  // 新建文件
  {
    "key": "cmd+f n",
    "command": "welcome.showNewFileEntries"
  },
  // 打开文件
  {
    "key": "cmd+f o",
    "command": "workbench.action.files.openFileFolder"
  },
  // 另存为文件
  {
    "key": "cmd+f e",
    "command": "workbench.action.files.saveAs"
  },
  // 保存文件
  {
    "key": "cmd+f s",
    "command": "workbench.action.files.save"
  },
  // 保存所有文件
  {
    "key": "cmd+f w",
    "command": "workbench.action.files.saveAll"
  },
  // 关闭文件
  {
    "key": "cmd+f q",
    "command": "workbench.action.closeActiveEditor"
  },
  // 关闭所有文件
  {
    "key": "cmd+f a",
    "command": "workbench.action.closeAllEditors"
  },
  // 切换侧边栏显示状态
  {
    "key": "cmd+n [",
    "command": "workbench.action.toggleSidebarVisibility"
  },
  // 显示文件资源管理器
  {
    "key": "cmd+n 1",
    "command": "workbench.files.action.focusFilesExplorer"
  },
  // 显示TODO Tree
  {
    "key": "cmd+n 2",
    "command": "todo-tree-view.focus"
  },
  // 显示全局搜索
  {
    "key": "cmd+n 3",
    "command": "workbench.action.replaceInFiles"
  },
  // 显示debug
  {
    "key": "cmd+n 4",
    "command": "workbench.view.debug",
    "when": "viewContainer.workbench.view.debug.enabled"
  },
  // 显示版本控制
  {
    "key": "cmd+n 5",
    "command": "workbench.view.scm",
    "when": "workbench.scm.active"
  },
  // 显示SQL Tools
  {
    "key": "cmd+n 6",
    "command": "workbench.view.extension.sqltoolsActivityBarContainer"
  },
  // 显示Docker
  {
    "key": "cmd+n 7",
    "command": "workbench.view.extension.dockerView"
  },
  // 显示测试
  {
    "key": "cmd+n 8",
    "command": "workbench.view.testing.focus"
  },
  // 显示插件商店
  {
    "key": "cmd+n 9",
    "command": "workbench.view.extensions",
    "when": "viewContainer.workbench.view.extensions.enabled"
  },
  // 切换面板显示状态
  {
    "key": "cmd+p [",
    "command": "workbench.action.togglePanel"
  },
  // 显示问题
  {
    "key": "cmd+p 1",
    "command": "workbench.panel.markers.view.focus"
  },
  // 显示输出
  {
    "key": "cmd+p 2",
    "command": "workbench.action.output.toggleOutput",
    "when": "workbench.panel.output.active"
  },
  // 显示终端
  {
    "key": "cmd+p 3",
    "command": "workbench.action.terminal.toggleTerminal",
    "when": "terminal.active"
  },
  // 显示调试控制台
  {
    "key": "cmd+p 4",
    "command": "workbench.debug.action.toggleRepl",
    "when": "workbench.panel.repl.view.active"
  },
  // 显示SQL CONSOLE
  {
    "key": "cmd+p 5",
    "command": "workbench.view.extension.sqltoolsPanelContainer"
  },
  {
    "key": "cmd+q",
    "command": "workbench.action.closeActiveEditor"
  },
  // 聚集在第一个选项卡中
  {
    "key": "cmd+e",
    "command": "workbench.action.focusFirstEditorGroup"
  },
  // 切换到上一个选项卡
  {
    "key": "cmd+,",
    "command": "workbench.action.previousEditor"
  },
  // 切换到下一个选项卡
  {
    "key": "cmd+.",
    "command": "workbench.action.nextEditor"
  },
  // 触发帮助提示
  {
    "key": "cmd+h",
    "command": "editor.action.showHover",
    "when": "editorTextFocus"
  },
  // 触发参数提示
  {
    "key": "cmd+j",
    "command": "editor.action.triggerParameterHints",
    "when": "editorHasSignatureHelpProvider && editorTextFocus"
  },
  {
    "key": "cmd+j",
    "command": "closeParameterHints",
    "when": "editorFocus && parameterHintsVisible"
  },
  // 触发建议提示
  {
    "key": "cmd+k",
    "command": "editor.action.triggerSuggest",
    "when": "editorHasCompletionItemProvider && textInputFocus && !editorReadonly"
  },
  {
    "key": "cmd+k",
    "command": "hideSuggestWidget",
    "when": "suggestWidgetVisible && textInputFocus"
  },
  // 触发快速修复
  {
    "key": "cmd+m",
    "command": "editor.action.quickFix",
    "when": "editorHasCodeActionsProvider && editorTextFocus && !editorReadonly"
  },
  {
    "key": "cmd+.",
    "command": "-editor.action.quickFix",
    "when": "editorHasCodeActionsProvider && editorTextFocus && !editorReadonly"
  },
  // 移动到下一个建议
  {
    "key": "cmd+n",
    "command": "selectNextSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
  // 移动到上一个建议
  {
    "key": "cmd+p",
    "command": "selectPrevSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
  // 放大字体
  {
    "key": "cmd+=",
    "command": "editor.action.fontZoomIn"
  },
  // 缩小字体
  {
    "key": "cmd+-",
    "command": "editor.action.fontZoomOut"
  },
  // 格式化代码
  {
    "key": "cmd+alt+l",
    "command": "editor.action.formatDocument",
    "when": "editorHasDocumentFormattingProvider && editorTextFocus && !editorReadonly && !inCompositeEditor"
  },
  // 选择全部, 在文本焦距点中失效
  {
    "key": "cmd+a",
    "command": "editor.action.selectAll",
    "when": "!editorTextFocus"
  },
  {
    "key": "cmd+a",
    "command": "-editor.action.selectAll"
  }
]
```

## 我的 init.vim

下面是我的~/.config/nvim/init.vim 文件，基本属于非常轻量级的配置了：

```bash
" 当打开nvim时，若没有下载vim-plug则自动下载
if empty(glob('~/.config/nvim/autoload/plug.vim'))
    silent !curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif


" 始终都会加载的配置项
let mapleader = "\<space>"
" 使用系统剪切板
set clipboard=unnamed

" 自动切换输入法
if executable('im-select')
    autocmd InsertLeave * :call system("im-select com.apple.keylayout.ABC")
    autocmd CmdlineLeave * :call system("im-select com.apple.keylayout.ABC")
endif

" ----- settings ----
if exists('g:vscode')
    " ----- Plug -----
    call plug#begin('~/.config/nvim/autoload/')
        " 快速跳转
        Plug 'asvetliakov/vim-easymotion'
        " 包裹修改
        Plug 'tpope/vim-surround'
    call plug#end()
	" 切换行注释
	nnoremap gc <Cmd>call VSCodeNotify('editor.action.commentLine')<CR>
	" 切换块注释
	nnoremap gC <Cmd>call VSCodeNotify('editor.action.blockComment')<CR>
	" 展开所有折叠
	nnoremap zR <Cmd>call VSCodeNotify('editor.unfoldAll')<CR>
	" 关闭所有折叠
	nnoremap zM <Cmd>call VSCodeNotify('editor.foldAll')<CR>
	" 展开当下折叠
	nnoremap zo <Cmd>call VSCodeNotify('editor.unfold')<CR>
	" 关闭当下折叠
	nnoremap zc <Cmd>call VSCodeNotify('editor.fold')<CR>
	" 切换当下折叠
	nnoremap zz <Cmd>call VSCodeNotify('editor.toggleFold')<CR>
	" 转到文件中上一个问题
	nnoremap g[ <Cmd>call VSCodeNotify('editor.action.marker.prevInFiles')<CR>
	" 转到文件中下一个问题
	nnoremap g] <Cmd>call VSCodeNotify('editor.action.marker.nextInFiles')<CR>
    " 用H替换掉^
    noremap H ^
    " 用L替换掉$
    noremap L $
    " 使用vscode的undo替换nvim的undo
	nnoremap u <Cmd>call VSCodeNotify('undo')<CR>
    " easymotion相关配置
    let g:EasyMotion_smartcase = 0
    " easymotion前缀 leader leader
    map <Leader> <Plug>(easymotion-prefix)
    " 其他键位绑定
    map <Leader>l <Plug>(easymotion-lineforward)
    map <Leader>j <Plug>(easymotion-j)
    map <Leader>k <Plug>(easymotion-k)
    map <Leader>h <Plug>(easymotion-linebackward)

else
    " 以正常模式启动nvim时加载的配置项
    " 显示行号
	set number
	" 设置相对行号
	set relativenumber
	" 设置行宽
	set textwidth=80
	" 设置自动换行
	set wrap
	" 是否显示状态栏
	set laststatus=2
	" 语法高亮
	syntax on
	" 支持鼠标
	set mouse=a
	" 设置编码格式
	set encoding=utf-8
	" 启用256色
	set t_Co=256
	" 开启文件类型检查
	filetype indent on
	" 设置自动缩进
	set autoindent
	" 设置tab缩进数量
	set tabstop=4
	" 设置>>与<<的缩进数量
	set shiftwidth=4
	" 将缩进转换为空格
	set expandtab
	" 自动高亮匹配符号
	set showmatch
	" 自动高亮匹配搜索结果
	set nohlsearch
	" 开启逐行搜索，也就是说按下一次按键就继续一次搜索
	set incsearch
	" 开启类型检查
	" set spell spelllang
	" 开启命令补全
	set wildmenu
	" 不创建备份文件
	set nobackup
	" 不创建交换文件
	set noswapfile
	" 多窗口下光标移动到其他窗口时自动切换工作目录
	set autochdir
    " ----- Plug -----
    call plug#begin('~/.config/nvim/autoload/')
        " 快速跳转
        Plug 'easymotion/vim-easymotion'
        " 包裹修改
        Plug 'tpope/vim-surround'
        " vim中文文档
        Plug 'yianwillis/vimcdoc'
        " 颜色插件
        Plug 'theniceboy/vim-deus'
        " 根据内容自动获取文件类型
        Plug 'Shougo/context_filetype.vim'
        " 自动进行注释
        Plug 'tyru/caw.vim'
        " 包裹修改
        Plug 'tpope/vim-surround'
        " 多光标模式操作
        Plug 'mg979/vim-visual-multi', {'branch': 'master'}
    call plug#end()
    " -- ctrl+/设置为打开、关闭注释
    if has('win32')
        nmap <C-/> gcc
        vmap <C-/> gcc
    else
        nmap <C-_> gcc
        vmap <C-_> gcc
    endif
    "按键映射前缀: <leader>v。
    let g:VM_maps = {}                 "取消默认按键映射。
    let g:VM_maps['Find Under'] = 'gb' "进入多光标模式并选中光标下字符串。
endif
```

# vim-plug

## 基本介绍

vim-plug 是一款 vim 插件，我们可以使用它来管理、使用、下载其他的 vim 插件，总而言之 vim 中可以用插件来管理插件。

上面的配置文件中已经设置了当打开 nvim 时自动安装 vim-plug：

```bash
" 当打开nvim时，若没有下载vim-plug则自动下载
if empty(glob('~/.config/nvim/autoload/plug.vim'))
    silent !curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif
```

该插件使用非常简单，记住以下几个命令即可：

```ruby
:PlugStatus
:PlugInstall
:PlugClean
:PlugUpdate
```

我们可在 github 或者[vimAwesome](https://vimawesome.com/)中搜索你需要的插件，并将它们安装在 nvim 中，安装方式如下所示，只需要在 init.vim 配置文件中加入以下选项即可：

```bash
" ----- Plug -----
call plug#begin('~/.config/nvim/autoload/')
    " 快速跳转
    Plug 'asvetliakov/vim-easymotion'
    " 包裹修改
    Plug 'tpope/vim-surround'
call plug#end()
```

然后你需要 x!后重新进入 nvim，并在 COMMAND 模式下输入:PlugInstall，它将自动安装所有插件。

如果想卸载某个插件，则直接将其进行注释即可，这样 nvim 在启动时就不会加载该插件了，如果想删除某个插件，则输入:PlugClean 命令，它将删除所有被注释后的插件。

## vim-easymotion

原生的 vim-easymotion 插件在 vscode-nvim 中会出现一些问题，因此 vscode-nvim 作者 fork 了一份[vim-easymotion](https://github.com/asvetliakov/vim-easymotion)，我们可直接对其进行安装。

在上面我的 init.vim 文件中都已经做好了一些基本配置，如下所示：

```xml
" 安装
call plug#begin('~/.config/nvim/autoload/')
    " 快速跳转
    Plug 'asvetliakov/vim-easymotion'
    " 包裹修改
    Plug 'tpope/vim-surround'
call plug#end()

" 键位绑定
" easymotion相关配置
let g:EasyMotion_smartcase = 0
" easymotion前缀 leader leader
map <Leader> <Plug>(easymotion-prefix)
" Gif config
map <Leader>l <Plug>(easymotion-lineforward)
map <Leader>j <Plug>(easymotion-j)
map <Leader>k <Plug>(easymotion-k)
map <Leader>h <Plug>(easymotion-linebackward)
```

原生的 easymotion 插件的使用是 leader leader 作为前缀，而该 fork 的 easymotion 插件则推荐只使用一个 leader，下面是它的使用方法：

```xml
<leader>w       ： 高亮所有词的词首字母
<leader>s<char> ： 全页搜索该字符
<leader>f<char> ： 光标以下搜索该字符
<leader>h<char> ： 光标以左搜索该字符
<leader>l<char> ： 光标以右搜索该字符
<leader>j<char> ： 光标以下搜索该字符
<leader>k<char> ： 光标以上搜索该字符
```

## vim-surround

vim-surround 插件是非常方便的一款插件，可以快速的修改包裹符号，我也对他进行了安装：

```bash
" 安装
call plug#begin('~/.config/nvim/autoload/')
    " 快速跳转
    Plug 'asvetliakov/vim-easymotion'
    " 包裹修改
    Plug 'tpope/vim-surround'
call plug#end()
```

使用也非常简单，如下所示：

```xml
ds<existing> 		  ： 将某个包裹修改为另一个包裹
cs<existing><desired> ： 删除某个包裹
```

示例如下：

```csharp
# 删除以下的[]
[1, 2, 3] -> ds[

# 将以下的[]修改为()
[1, 2, 3] -> cs[(
```

# 其他问题

## 自动切换输入法

如果你在 INSERT 模式下使用中文输入法进行编辑，当 ESC 到 NORMAL 模式下后，它将依然保持中文输入法，这会使我们需要频繁的使用 ctrl+shift 或者 cmd+space 进行输入法切换，非常麻烦。

为了解决这个问题，你必须先在你的计算机上安装一个[im-select](https://github.com/daipeihust/im-select)脚本：

```perl
$ curl -Ls https://raw.githubusercontent.com/daipeihust/im-select/master/install_mac.sh | sh
```

这个脚本有 2 个作用，当你输入 im-select 后它将获取当前输入法，当你输入 im-select xxx 后它将切换至 xxx 输入法。

首先你需要先切换到英文输入法中到终端执行 im-select 命令，并把结果保存下来：

```csharp
$ im-select
com.apple.keylayout.ABC
```

然后再到 init.vim 中输入以下代码即可，我上面也已经进行了解决：

```perl
" 自动切换输入法
if executable('im-select')
    autocmd InsertLeave * :call system("im-select com.apple.keylayout.ABC")
    autocmd CmdlineLeave * :call system("im-select com.apple.keylayout.ABC")
endif
```

## 可视模式的选择

vscode-nvim 插件中的 VISUAL 模式并非真正的 vscode 选择，这是我对比 vscode-vim 插件来说对 vscode-nvim 插件所最不能接受的一个点，因为我会使用很多翻译插件来进行源代码注释翻译，如[google-translate](https://marketplace.visualstudio.com/items?itemName=hancel.google-translate)插件或者[comment-translate](https://marketplace.visualstudio.com/items?itemName=intellsmi.comment-translate)。

尽管 vscode-nvim 提供了以下热键来将 VISUAL 模式下的选择变更为 vscode 选择，但是仍然会出现一些问题，它仍然不是选择全部的 VISUAL 行：

```dos
f1
ctrl+shift+p
cmd+shift+p
```

后续希望作者能够将这点进行改进吧，也呼吁大家去提一些 issues。

## 查看所有的错误

当使用 vscode-nvim 或者 vscode-vim 插件时，[Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)这款 vscode 插件总能给你带来意外的惊喜，让你更加直观的看到当前页中所有代码的问题、警告、错误信息。

如下图所示：

![image-20211108022237711](https://images-1302522496.cos.ap-nanjing.myqcloud.com/img/image-20211108022237711.png)

它可能会影响 vscode-nvim 的性能，但我仍然十分依赖它。

## 插件与性能损失

如果您有任何性能问题（通常是光标抖动），请确保您没有使用这些类型的扩展：

1）任何经常渲染装饰器的东西：

- 行号扩展（VSCode 内置了对普通/相对行号的支持）
- 缩进指南扩展（VSCode 有内置的缩进指南）
- 括号荧光笔扩展（VSCode 有内置功能）

2）延迟扩展主机的 VSCode 扩展，如“Bracket Pair Colorizer”

3）增加延迟并导致性能问题的 VIM 插件。

- 确保禁用不需要的插件，因为其中许多插件对 vscode 没有意义并且可能会导致问题。
- 您不需要任何代码、突出显示、完成、lsp 插件以及任何生成窗口/缓冲区（nerdtree 和类似的）、模糊查找器等的插件。
- 许多导航/文本对象/编辑插件应该没问题。

如果不确定，请禁用所有其他扩展且重新加载 vscode window，并在报告之前查看问题是否仍然存在。

## 内置定义的热键

下面这些命令都是由 vscode-nvim 插件调用 vscode 中的命令完成的，仅限于 COMMAND 模式下使用，这些命令在原生的 nvim 中也都适用，不同的是它们并不是调用的原生命令接口。

1）文件管理命令：

| 命令  | 描述                           |
| ----- | ------------------------------ |
| :ene  | 创建新的无标题文件             |
| :e    | 快速打开文件                   |
| :find | 按名称搜索文件                 |
| :sav  | 另存为文件                     |
| :w    | 保存当前文件                   |
| :q    | 关闭所有编辑器                 |
| :wq   | 保存当前文件，并关闭活动编辑器 |
| :x    | 保存当前文件，并关闭活动编辑器 |
| :qa   | 保存所有文件，并关闭所有编辑器 |
| :wa   | 保存所有文件，不关闭任何编辑器 |
| :wqa  | 保存所有文件，并关闭所有编辑器 |
| :xa   | 保存所有文件，并关闭所有编辑器 |

2）标签相关命令：

| 命令      | 描述               |
| --------- | ------------------ |
| :tabnew   | 创建新的无标题文件 |
| :tabe     | 快速打开文件       |
| :tabf     | 按名称搜索文件     |
| :tabc     | 关闭当前的的编辑器 |
| :tabo     | 关闭其他所有编辑器 |
| :tabp     | 切换到上一个编辑器 |
| :tabn     | 切换到下一个编辑器 |
| :tabfirst | 切换到第一个编辑器 |
| :tablast  | 切换到末一个编辑器 |

3）窗口相关命令：

| 命令  | 描述                                 |
| ----- | ------------------------------------ |
| :sp   | 水平拆分编辑器                       |
| :vsp  | 垂直拆分编辑器                       |
| :new  | 水平拆分编辑器，并创建新的无标题文件 |
| :vnew | 垂直拆分编辑器，并创建新的无标题文件 |
| :only | 关闭所有的编辑器                     |

下面这些热键都是由 vscode-nvim 插件调用 vscode 中的命令完成的，原生 nvim 中不存在下面大多数热键。

1）代码编辑区热键：

| 热键（1） | 热键（2） | 功能描述       |
| --------- | --------- | -------------- |
| =         | ==        | 格式化选定内容 |
| gh        | K         | 显示悬停       |
| gd        | C-]       | 转到定义       |
| gf        | 无        | 转到声明       |
| gH        | 无        | 查看引用       |
| gO        | 无        | 转到符号       |
| C-W gd    | C-W gf    | 打开侧边的定义 |
| gD        | 无        | 速览定义       |
| gF        | 无        | 查看声明       |
| tab       | 无        | 切换小部件焦点 |
| C-p       |           | 切换上一个建议 |
| C-n       |           | 切换下一个建议 |

2）一些 INSERT 模式下定义的热键：

| 热键                  | 描述                                       |
| --------------------- | ------------------------------------------ |
| C-r [0-9a-z"%#*+:.-=] | 从寄存器粘贴内容                           |
| C-a                   | 插入之前插入的内容                         |
| C-o                   | 切换到 NORMAL 模式中执行一个命令，然后返回 |
| C-u                   | 删除当前光标当行首前的所有内容             |
| C-w                   | 删除左侧的单词                             |
| C-h                   | 删除左侧的字符                             |
| C-t                   | 光标向右移动，单位是一个缩进               |
| C-d                   | 光标向左移动，单位是一个缩进               |
| C-j                   | 插入新行                                   |
| C-c                   | 退出 INSTER 模式                           |

3）一些侧边栏中进行移动的热键：

| 热键      | 描述             |
| --------- | ---------------- |
| j / k     | 向下向上移动光标 |
| h / l     | 打开或关闭项目   |
| Enter     | 打开项目         |
| gg        | 返回顶部         |
| G         | 返回底部         |
| o         | 切换展开         |
| C-u / C-d | 向上或向下翻页   |
| Escape    | 切换键盘导航     |

4）文件资源管理的热键：

| 热键 | 描述                     |
| ---- | ------------------------ |
| r    | 重命名文件               |
| d    | 删除文件                 |
| y    | 拷贝文件                 |
| x    | 剪切文件                 |
| p    | 粘贴文件                 |
| v    | 以垂直拆分的方式打开文件 |
| a    | 创建新文件               |
| S-a  | 创建新目录               |

此外还有一些热键这里没有例举出来，可参阅官方文档进行查看。
