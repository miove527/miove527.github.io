---
title: 建站参考
description: Hugo Stack
date: 2025-05-10 00:00:00+0000
draft: false
# image: lib.jpg
mindmap: true
translationKey: "build-ref"
tags: 
    - markdown
    - base
categories:
    - syntax
---

# 建站基础
- https://github.com/CaiJimmy/hugo-theme-stack
- https://github.com/CaiJimmy/hugo-theme-stack-starter
    - `Use this template` 直接创建一个新的仓库：`<username>.github.io`
    - 创建Codespace
    - 手动微调
    - Settings -> Pages：Change the build branch from master to gh-pages
- deploy automatically

## 微调
- 新文件夹`layouts`
- `config\`
    - `config.toml`
        - `baseURL`
        - `languageCode`
        - `title`
        - `defaultContentLanguage`
        - `hasCJKLanguage`
        - `disqusShortname`
            - disabled
    - `params.toml`
        - `[sidebar]`
            - emoji
            - subtitle
                - `\layouts\partials\sidebar\left.html`: `<h2 class="site-description">`
        - `[sidebar.avatar]`: src
        - `math`
        - `[footer]`
    - `_languages.toml` -> `languages.toml`
        - `title`
    - `menu.toml`
        - `[[social]]`
        - `RSS`
- `page`
- `README.md`
- pic
    - `assets\img\avatar.jpg`
    - `\static\favicon.png`
- language switcher
    - add ref

## 文件夹架构
```
.github.io
├── .devcontainer
├── .github
│   └── workflows
├── .vscode
├── assets
│   ├── jsconfig.json
│   ├── img
│   │   └── avatar.jpg
│   └── scss
│       └── custom.scss
├── config
│   └── _default
│       ├── config.toml
│       ├── languages.toml
│       ├── markup.toml
│       ├── menu.en.toml
│       ├── menu.zh-cn.toml
│       ├── module.toml
│       ├── params.toml
│       ├── permalinks.toml
│       └── related.toml
├── content
│   ├── _index.en.md
│   ├── _index.zh-cn.md
│   ├── categories
│   │   └── _index.md
│   ├── page
│   │   ├── archives
│   │   │   ├── index.en.md
│   │   │   └── index.zh-cn.md
│   │   ├── search
│   │   │   ├── index.en.md
│   │   │   └── index.zh-cn.md
│   │   ├── links
│   │   │   ├── index.en.md
│   │   │   └── index.zh-cn.md
│   │   └── about
│   │       ├── index.en.md
│   │       └── index.zh-cn.md
│   └── post
│       └── 250510_first
│           ├── lib.jpg
│           ├── index.en.md
│           └── index.zh-cn.md
├── layouts
│   ├── 404.html
│   ├── index.html
│   ├── _default
│   ├── page
│   ├── partials
│   └── shortcodes
├── static
│   ├── favicon.png
│   ├── css
│   │   ├── kityminder.core.css
│   │   └── mindmap.css
│   └── js
│       ├── kity.min.js
│       ├── kityminder.core.min.js
│       └── mindmap.js
├── .gitignore
├── go.mod
├── go.sum
├── LICENSE
└── README.md
```

# Front Matter

```yaml
---
title: xx
description: xx
date: 2025-05-10 00:00:00+0000
draft: false
image: xx.jpg
mindmap: true
translationKey: "xx"
tags: 
    - markdown
    - base
categories:
    - syntax
---
```

# 思维导图
主要参考：http://www.9ong.com/082020/hugo%E6%94%AF%E6%8C%81%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.html#frontmatter%E8%AE%BE%E7%BD%AEmindmap

## 步骤
- Front Matter 设置：`mindmap: true`
- 文件下载
    - https://github.com/fex-team/kityminder-core
    - https://github.com/HunterXuan/unordered-list-to-mind-map
- 引入：`.github.io\layouts\partials\article\components\content.html`
    - jQuery
    - css
    - js
- 创建html：`.github.io\layouts\shortcodes`
    - mind-md.html
    - mind-lg.html
    - mind-sm.html


```html
<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
<script src="{{.Site.BaseURL}}js/kity.min.js"></script>
<script src="{{.Site.BaseURL}}js/kityminder.core.min.js"></script>
<script src="{{.Site.BaseURL}}js/mindmap.js"></script>
<link href="{{.Site.BaseURL}}css/kityminder.core.css" rel="stylesheet">
<link href="{{.Site.BaseURL}}css/mindmap.css" rel="stylesheet">
```

```html
<div id="{{ .Get 0 }}" class="mindmap mindmap-md">
    {{- .Inner | markdownify -}}
</div>
```

```html
<div id="{{ .Get 0 }}" class="mindmap mindmap-lg">
    {{- .Inner | markdownify -}}
</div>
```

```html
<div id="{{ .Get 0 }}" class="mindmap mindmap-sm">
    {{- .Inner | markdownify -}}
</div>
```

## 使用示例

{{< mind-md mymindmap >}}
- 思维导图标题
    - 一级节点
        - 二级节点1
        - 二级节点2
    - 另一个一级节点
        - 二级节点3
{{< /mind-md >}}

```
{{</* mind-md mymindmap */>}}
- 思维导图标题
    - 一级节点
        - 二级节点1
        - 二级节点2
    - 另一个一级节点
        - 二级节点3
{{</* /mind-md */>}}
```
