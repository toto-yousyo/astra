---
title: Create mdfile include frontmatter
excerpt: I want write blog contents everyday, so I start easy writing.
image: '~/assets/images/desert-9019840_1280.jpg'
tags:
  - astro
  - markdown
---

# Create mdfile include frontmatter

NeovimでMarkdown（.md）ファイルを作成する際に、フロントマターが自動的にテンプレートとして挿入されるように設定するには、Neovimのプラグインや`autocmd`を使用する方法があります。

### 手順1: `autocmd`を使った設定

Neovimの標準機能である`autocmd`を使用すれば、新しい`.md`ファイルを開いた際にフロントマターを自動挿入できます。以下の手順に従って、設定を行います。

#### 1\. `init.vim`または`init.lua`に設定を追加する

Neovimの設定ファイルに以下の設定を追加します。`init.vim`を使っている場合と、`init.lua`を使っている場合で書き方が異なるので、適切な方を選んでください。

##### `init.lua`の場合:

```lua
vim.api.nvim_create_autocmd("BufNewFile", {
  pattern = "*.md",
  command = "0r ~/.config/nvim/templates/markdown_frontmatter.md",
})
```

#### 2\. テンプレートファイルを作成

次に、テンプレートとなるMarkdownのフロントマターを別ファイルとして作成します。例えば、以下の内容を含んだテンプレートファイルを`~/.config/nvim/templates/markdown_frontmatter.md`として保存します。

```yaml
---
title: ''
date: '{{ date }}'
tags: []
---
```

`date`の部分など、動的に挿入したい場合には後述するプラグインの導入が必要です。
