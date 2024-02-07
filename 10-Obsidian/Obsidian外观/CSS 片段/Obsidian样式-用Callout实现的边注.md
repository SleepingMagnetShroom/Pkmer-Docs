---
uid: 20240120143852
title: Obsidian 样式 - 用 Callout 实现的边注（注释）
tags: [obsidian, css, callout, 边注, 注释]
description: 在 obsidian 中优雅的实现边注
author: Huajin
type: other
draft: false
editable: false
modified: 20240201185324
---

# Obsidian 样式 - 用 Callout 实现的边注（注释）

使用 obsidian 自带的 callout 语法，不污染笔记，且用法非常简单。

![image.png](https://cdn.pkmer.cn/images/20240120155800.png!pkmer)

<center>主题：Obsidian Nord</center>

> [!tip] 另外
> 内容修改自 discord 两位大佬分享的 css，使得实时模式可以显示，并适配多种主题。[参考一](https://discord.com/channels/686053708261228577/702656734631821413/1155147566615367680), [参考二](https://discord.com/channels/686053708261228577/702656734631821413/1073456247849881610).

## 使用方法

添加好 css 后，在正文中直接用边注的 callout 即可（如何添加 css 可以看这篇：[[Obsidian的CSS代码片段]]）

如果想要左边的边注，可以这样写

```
> [!NOTE|aside-l] 左侧注释
> 注释内容
```

如果想要右边的边注，可以这样写

```
> [!NOTE|aside-r] 右侧注释
> 注释内容
```

如果想要边注可折叠，可以用 callout 的折叠语法

```
> [!NOTE|aside-l]+ 默认展开的注释
> 注释内容
```

```
> [!NOTE|aside-r]- 默认折叠的注释
> 注释内容
```

边注基于 callout 实现，因此可以像 callout 一样使用不同的样式

```
> [!ERROR|aside-l] ERROR 样式
> 还可以用别的callout，例如important, cite 等
```

## Style Settings

本 css 可以配合 style settings 插件使用，可以调整注释的样式

![image.png](https://cdn.pkmer.cn/images/20240128015408.png!pkmer)

例如：开启背景 and 边注标题置于顶部 and 去除 folder icon

![image.png](https://cdn.pkmer.cn/images/20240128015300.png!pkmer)

## 部分主题截图展示

默认主题：

![image.png](https://cdn.pkmer.cn/images/20240128015757.png!pkmer)

Anuppuccin：

![image.png](https://cdn.pkmer.cn/images/20240128015938.png!pkmer)

blue-topaz:

![image.png](https://cdn.pkmer.cn/images/20240128020040.png!pkmer)

minimal:

![image.png](https://cdn.pkmer.cn/images/20240128020251.png!pkmer)

## 注意

- 渲染的是 callout，没有添加新语法，方便迁移以及修改。
- 边注的标题在侧边时不宜过长
- 边注需要显示在侧面，如果屏幕过小，可能无法全部显示。例如手机屏幕、页面分页
- 暂未导出 PDF 的样式（doing）
- 边注的位置目前只能固定在侧边，还未支持上下调整，后续可能会增加该功能（doing）

## CSS 源码

如果样式遇到问题，试着更新下代码。文章中的代码不一定是最新的，可以在 [xhuajin/obsidian-sidenote-callout(github.com)](https://github.com/xhuajin/obsidian-sidenote-callout) 查看更新最新代码。

如果代码使用遇到问题，请检查：

- css 是否放置到正确的文件夹，是否在库中启用
- 你在笔记中写的 callout 是否正确
- 如果没有显示，是不是屏幕可显示的范围过小

遇到 BUG：可以在上方链接中给我提 issue，或者在 pkmer 的 q 群问问。

希望增加功能：在上方链接中给我提 issue

> 更新日志
> 2024-01-22：可以直接修改边注与正文之间的距离、边注标题位于顶部时，可以自行选择标题的位置（居左、居中和居右）

```css
/*
author: Huajin
reference: 
  https://discord.com/channels/686053708261228577/702656734631821413/1155147566615367680
  https://discord.com/channels/686053708261228577/702656734631821413/1073456247849881610
*/

/* @settings

name: SideNote Callout
id: sidenote-callout
settings:
  -
    id: top-sidenote-callout-title
    title: Sidenote Callout Title to Top
    title.zh: 边注标题置于顶部
    type: class-toggle
    default: false
  -
    id: top-left-sidenote-callout-title-position
    title: Top Left Sidenote Callout Title Position
    title.zh: 标注在顶部时，左边注的标题位置
    type: class-select
    default: l-center-callout-title
    options:
      -
        label: left
        value: l-left-callout-title
      -
        label: center
        value: l-center-callout-title
      -
        label: right
        value: l-right-callout-title
  -
    id: top-right-sidenote-callout-title-position
    title: Top Right Sidenote Callout Title Position
    title.zh: 标注在顶部时，右边注的标题位置
    type: class-select
    default: r-center-callout-title
    options:
      -
        label: left
        value: r-left-callout-title
      -
        label: center
        value: r-center-callout-title
      -
        label: right
        value: r-right-callout-title
  -
    id: sidenote-backgound
    title: Callout Background
    title.zh: Callout背景
    description: enable background color
    description.zh: 开启背景色
    type: class-toggle
    default: false
  -
    id: aside-width
    title: sidenote width
    title.zh: 边注宽度
    description: The width of the sidenote
    description.zh: 边注的宽度
    type: variable-number-slider
    default: 200
    min: 150
    max: 300
    step: 10
    format: px
  -
    id: aside-offset
    title: sidenote offset
    title.zh: 边注与正文的间距
    description: The offset between the sidenote and the text(default is 16px)
    description.zh: 边注与正文的间距(默认为16px)
    type: variable-number-slider
    default: 16
    min: 0
    max: 50
    step: 1
    format: px
  -
    id: hide-sidenote-callout-fold-icon
    title: Hide Fold Icon
    title.zh: 隐藏折叠图标
    type: class-toggle
    default: false
*/

body {
  --aside-width: 200px;
  --aside-offset: var(--size-4-4);
  --line-width: var(--file-line-width, --line-width);
}

.markdown-source-view.mod-cm6 .cm-content > .cm-callout:has(.callout[data-callout-metadata*="aside"])[contenteditable=false]  {
  contain: none !important;
  overflow: visible;
}

.markdown-source-view.mod-cm6 .cm-content > .cm-callout:has(.callout[data-callout-metadata*="aside"])[contenteditable=false]>.markdown-rendered {
  overflow: visible;
}

.cm-callout:has(.callout[data-callout-metadata*="aside"])[contenteditable=false]>.edit-block-button {
  display: none;
}

:is(.markdown-source-view .cm-callout, div:not([class])):has(> .callout[data-callout-metadata*="aside"]) {
  position: relative;
  overflow: visible;
}

.callout[data-callout-metadata*="aside"] {
  /* --aside-offset: var(--size-4-4); */
  position: absolute;
}

.callout[data-callout-metadata*="aside-l"] {
  left: calc(-1 * (var(--aside-width) + var(--aside-offset)));
  right: calc(100% + var(--aside-offset));
}

.callout[data-callout-metadata*="aside-r"] {
  left: calc(var(--file-line-width) + var(--aside-offset));
  right: calc(-1 * var(--aside-width));
}

/* .markdown-reading-view .callout[data-callout-metadata*="aside-l"] {
  left: calc(50vw - var(--file-line-width)/2 - var(--aside-width) - 2 * var(--aside-offset));
  right: calc(50vw + var(--file-line-width)/2);
}

.markdown-reading-view .callout[data-callout-metadata*="aside-r"] {
  left: calc(var(--file-line-width) + 2 * var(--aside-width));
  right: calc(50vw - var(--file-line-width)/2 - var(--aside-width) - 2 *var(--aside-offset));
}   */

@media (hover: hover) {
  .markdown-source-view.mod-cm6 .cm-embed-block:has(> div > [data-callout-metadata*="aside"]):hover {
    overflow: visible;
  }
  .markdown-source-view.mod-cm6 .cm-embed-block:not(.cm-table-widget):hover {
    box-shadow: unset;
  }
}

/* ------------ */

.callout[data-callout-metadata*="aside"] {
  --block-spacing: 0.75rem;
  --speaker-block-width: 20%;
  margin: 0px;
  padding: 0px;
  display: grid;
  background-color: var(--background-primary) !important;
  border: none;
}

.sidenote-backgound .callout[data-callout-metadata*="aside"] {
  background-color: rgba(var(--callout-color), 0.1) !important;
  padding-bottom: 10px;
}
.callout[data-callout-metadata*="aside"] {
  grid-template-columns: var(--speaker-block-width) calc(100% - var(--speaker-block-width));
}

.top-sidenote-callout-title .callout[data-callout-metadata*="aside"] {
  grid-template-columns: unset;
}

.callout[data-callout-metadata*="aside"] .callout-title {
  height: calc(100% - var(--block-spacing));
  text-align: right;
  word-wrap: break-word;
  border-right: 3px solid;
  border-bottom: unset;
  flex: 1 0 auto;
  color: rgb(var(--callout-color)) !important;
  background-color: unset !important;
  /* padding-right: var(--block-spacing); */
  padding: 0;
}
.callout[data-callout-metadata*="aside"] .callout-title {
  display: inline-block;
}

body:not(.top-sidenote-callout-title) .setting-item[data-id="top-left-sidenote-callout-title-position"],
body:not(.top-sidenote-callout-title) .setting-item[data-id="top-right-sidenote-callout-title-position"] {
  display: none;
}

.top-sidenote-callout-title .callout[data-callout-metadata*="aside"] .callout-title {
  display: flex;
  flex-direction: row-reverse;
}

.callout[data-callout-metadata*="aside"] .callout-title-inner {
  font-weight: var(--bold-weight) !important;
  color: rgb(var(--callout-color)) !important;
  width: 1ch;
  margin: 0 auto;
  text-align: left;
}

.top-sidenote-callout-title .callout[data-callout-metadata*="aside"] .callout-title-inner {
  margin: 0 auto;
  width: unset;
}

.callout[data-callout-metadata*="aside"] .callout-title-inner {
  margin: 0 auto;
}

.l-left-callout-title .callout[data-callout-metadata*="aside-l"] .callout-title-inner {
  margin-left: var(--block-spacing);
}

.l-right-callout-title .callout[data-callout-metadata*="aside-l"] .callout-title-inner {
  margin: 0 var(--block-spacing);
}

.r-left-callout-title .callout[data-callout-metadata*="aside-r"] .callout-title-inner {
  margin-left: var(--block-spacing);
}

.r-right-callout-title .callout[data-callout-metadata*="aside-r"] .callout-title-inner {
  margin: 0 var(--block-spacing);
}

.callout[data-callout-metadata*="aside"]>* {
  margin-top: var(--block-spacing);
}

.callout[data-callout-metadata*="aside"]>.callout-title>.callout-icon {
  display: none;
}

.hide-sidenote-callout-fold-icon .callout[data-callout-metadata*="aside"] .callout-fold {
  display: none;
}

.callout[data-callout-metadata*="aside"]>.callout-title>.callout-fold,
.callout[data-callout-metadata*="aside"]>.callout-title>.callout-fold.is-collapsed {
  padding: 0;
  justify-content: center;
}

.callout[data-callout-metadata*="aside"]>.callout-content {
  padding: 0px var(--block-spacing);
  border-top: unset;
  max-height: 200px;
  overflow-y: auto;
}

.callout[data-callout-metadata*="aside"]>.callout-content>p:first-child {
  margin-top: 0px;
}

.callout[data-callout-metadata*="aside"]>.callout-content>p:last-child {
  margin-bottom: 0px;
}

.callout[data-callout-metadata*="aside"]>.callout-content::-webkit-scrollbar-thumb {
  width: 11px;
  height: 11px;
  background-color: transparent !important;
}

.callout[data-callout-metadata*="aside"]>.callout-content:hover::-webkit-scrollbar-thumb {
  background-color: var(--scrollbar-thumb-bg) !important;
}

/* ------- */

@media print {
  .callout[data-callout-metadata*="aside-l"] {
    left: 0;
    right: calc(100% - var(--aside-width));
  }
  .callout[data-callout-metadata*="aside-r"] {
    left: calc(100% - var(--aside-width));
    right: 0;
  }
  div:not(.callout-content)>p {
    width: calc(100% - 2 * var(--aside-width));
    margin: 0 auto;
  }
}
```

## 结合其他插件便捷添加边注
效果演示：![未命名 - 笔记 - Obsidian v1 5 3 2024-02-07 17-49-13](https://github.com/PKM-er/Pkmer-Docs/assets/144025083/7c936469-c00b-403b-bce4-747fb160de08)

- 需要使用到的插件
  - templater
  - commander
 
### step1 添加templater自动化模版
添加自动化模版基本步骤可参考[[Templater 实现关键词自动化套用模板]] [b站up Chris的视频](https://www.bilibili.com/video/BV1c64y1W7c2)
新建便捷添加边注的模版文件，命名为“Insert Callouts” .
将下面的内容添加至文件内（注：直接复制文本，不需要含有代码框，此处含有代码框是为了与正文区分）
```
<%*
const callouts = {
note: '🌐 ✏ Note',
info: '🔵 ℹ Info',
todo: '🔵 🔳 Todo',
tip: '🌐 🔥 Tip / Hint / Important',
abstract: '🌐 📋 Abstract / Summary / TLDR',
question: '🟡 ❓ Question / Help / FAQ',
quote: '🔘 💬 Quote / Cite',
example: '🟣 📑 Example',
success: '🟢 ✔ Success / Check / Done',
warning: '🟠 ⚠ Warning / Caution / Attention',
failure: '🔴 ❌ Failure / Fail / Missing',
danger: '🔴 ⚡ Danger / Error',
bug: '🔴 🐞 Bug',
};

const p = await tp.system.suggester(["正文","左侧","右侧"], [" ", "|aside-l", "|aside-r"], true, 'Select callout position.'); 
const type = await tp.system.suggester(Object.values(callouts), Object.keys(callouts), true, 'Select callout type.');


const title = await tp.system.prompt('Title:', '', true);
let content = await tp.system.prompt('Content (New line -> Shift + Enter):', '', true, true);
content = content.split('\n').map(line => `> ${line}`).join('\n')  

const calloutHead = `> [!${type}${p}]+ ${title}\n`;

tR += calloutHead + content + '\n' + '\n'
-%>
```

### step2 在Template HotKeys添加快捷指令
打开设置，选择templater插件，下拉找到“Template HotKeys”，点击“Add new hotkey for templater”, 选择模版为刚刚新建的“Insert Callouts.md”。在这里就可以直接添加快捷键。

### step3 使用commander添加快捷按钮
不习惯使用快捷键或者快捷键已经很拥挤了，可以使用commander模组添加快捷按钮。这里简单介绍在右键菜单中添加快捷按钮。
安装好commander以后，在任意文件编辑界面右键，点击右键菜单下面的“添加命令”，选择“Templater: 模版文件路径/Insert Callouts.md”, 重命名并保存。
