---
title: Creaate multi entry point for LP develop
excerpt: How to create multi LP pages and build to specific named folder
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
ags:
  - vire
  - react
---

# Create multi entry point for LP develop

### 手順概要

1.  **Viteのマルチエントリーポイントを使用**: 複数のLPのエントリーポイントを設定。
2.  **共通コンポーネントの再利用**: 各LPが共通コンポーネントを利用できるようにする。
3.  **特定のLPをビルドするための設定**: ビルド時に特定のエントリーポイント（LP）だけを生成する設定。

### 2\. 複数のLP用のエントリーポイントを作成

各LPごとにエントリーファイルを作成します。例えば、`src`フォルダに以下のようにエントリーファイルを作成します。

```css
src/
  └── lp1/
      └── main.jsx
  └── lp2/
      └── main.jsx
```

それぞれのエントリーポイントでは、Reactアプリをレンダリングします。

`src/lp1/main.jsx`:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App'; // LP1用のAppコンポーネント

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

`src/lp2/main.jsx`も同様に設定できます。

### 3\. 共通コンポーネントを使用

共通のコンポーネントは、`src/components/`のようなフォルダを作成し、その中に保存します。

例えば、共通のヘッダーコンポーネントを作成します。

`src/components/Header.jsx`:
これを各LPの`App.jsx`内でインポートして使用します。

### 4\. Viteの設定でマルチエントリーポイントを設定

Viteの設定ファイルで、複数のエントリーポイントを設定します。

`vite.config.js`:

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react-swc';
import tailwindcss from 'tailwindcss'; // resolve関数をインポート
import { resolve } from 'path';
// https://vitejs.dev/config/
const lp = process.env.LP || 'lp1'; // LPを環境変数で選択
export default defineConfig({
  server: {
    host: true,
    watch: {
      usePolling: true,
    },
  },
  base: 'https://ec-force.s3.amazonaws.com/aotsubucojp/uploads/assets/',
  build: {
    assetsDir: `${lp}`,
    rollupOptions: {
      input: {
        [lp]: resolve(__dirname, `src/${lp}/main.tsx`),
      },
    },
  },
  plugins: [react()],
  css: {
    postcss: {
      plugins: [tailwindcss()],
    },
  },
});
```

例えば、LP=lp1 npm run buildのように実行すれば、lp1だけがビルドされます。

### 5\. 特定のLPだけをビルドする

zshrcに下記のaliasを設置する

```zsh
alias bd='(){LP=$1 npm run build}'
```

呼び出す時はbd {lpのフォルダ名} で希望のフォルダ名のフォルダがdistに出来上がっている。
