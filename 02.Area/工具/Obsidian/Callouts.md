---
标题: Callouts
aliases: 
tags:
  - 学习/Obsidian
创建时间: 2025-07-18 12:07
修改时间: 2025-08-06 21:56
---
## 一、介绍

> 在 Obsidian 最新更新的14.0版本中，终于上线了“Callout”功能

启用callout模块: `> [!INFO]`

```Markdown
> [!INFO]
> 这里是callout模块
> 支持 **markdown** 和 [[Internal link|wikilinks]]
```

> [!info]
> 这里是callout模块
> 支持 **markdown** 和 [[Internal link|wikilinks]]

## 二、样式

> 默认有12种风格，每一种有不同的颜色和图标

- note
- abstract、summary、tldr
- info、todo
- tip、hint、important
- success、check、done
- question、help、faq
- warning、caution、attention
- failure、fail、missing
- danger、error
- bug
- example
- quote、cite

> [!note] note - 备忘、笔记、注解

> [!abstract] abstract、summary、tldr - 摘要

> [!info] info - 信息、资讯

> [!todo] todo - 待办

> [!tip] tip、hint、important - 提示、重要的

> [!success] success、check、done - 成功、检查，核对、完成

> [!question] question、help、faq - 问题、帮助、常见问题，解答

> [!warning] warning、caution、attention - 警告、提醒、注意

> [!failure] failure、fail、missing - 失败、失效、缺失

> [!danger] danger、error - 危险、错误

> [!bug] bug - 错误、漏洞

> [!example] example - 举例、示例

> [!quote] quote, cite - 引用

## 三、标题和内容

> 可以自定义标题，然后直接不要主体部分

```markdown
> [!TIP] Callouts can have custom titles, which also supports **markdown**!
```

> [!TIP] Callouts can have custom titles, which also supports **markdown**!

## 四、折叠

> 可以使用 `+` 默认展开或者 `-` 默认折叠正文部分

```markdown
> [!FAQ]- Are callouts foldable?
> Yes! In a foldable callout, the contents are hidden until it is expanded.
```

显示为:

> [!FAQ]- Are callouts foldable?
> Yes! In a foldable callout, the contents are hidden until it is expanded.

## 五、自定义

> Callout的类型和图标是用CSS来描述，颜色是`r, g, b` 三色组，图标有相应的 icon ID (比如`lucide-info`)，也可以自定义SVG图标

```css
.callout[data-callout="my-callout-type"] {
    --callout-color: 0, 0, 0;
    --callout-icon: icon-id;
    --callout-icon: '<svg>...custom svg...</svg>';
}
```

参考自英文文档：[Use callouts](https://help.obsidian.md/callouts)
