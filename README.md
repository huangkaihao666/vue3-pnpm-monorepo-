---
theme: z-blue
---

**前言**

在现代前端开发中，Vue 3 已经成为构建高效、可维护用户界面的首选框架之一。随着项目规模的扩大，如何有效地管理多个相关项目成为一个重要课题。`Monorepo（单一代码库）作为一种解决方案，允许我们在一个代码库中管理多个项目，从而提高代码复用性和开发效率`。

本文将带你一步步`使用 Vue 3 和 pnpm 搭建一个 Monorepo 项目`。pnpm 是一个快速、节省磁盘空间的包管理器，特别适合在 Monorepo 环境中使用。通过本文的学习，你将掌握如何在一个代码库中管理多个 Vue 3 项目，并利用共享库来提高代码复用性。

## 准备工作

在开始搭建 Vue 3 + pnpm 的 Monorepo 项目之前，我们需要确保环境已经准备好。以下是准备工作的步骤：

1.  安装 `Node.js`
    首先，确保你已经安装了 Node.js。你可以通过以下命令检查 Node.js 是否已经安装：

```js
node -v
```

如果你还没安装 node ,需要到官网安装一下，基本上是傻瓜式安装，不过考虑到`我们要使用到 pnpm ，建议安装node18版本及以上`， 我们后续安装 pnpm 时，需要确保 Node.js 的版本至少为 18 ，因为 pnpm9 需要使用 node18 才能正常工作。‌不过也可以使用 nvm 对 node 版本进行管理，方便使用，node 和 nvm 官网如下，可根据自己的需要下载：

*   [node.js官网](https://nodejs.org/zh-cn)
*   [ nvm官网](https://nvm.uihtm.com/)

2.  安装 `pnpm`

```js
npm i pnpm -g
```

效果如下:

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/7523a61ca19d4ac2b7b0c02c424c7a0f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=R7bTBR1Yv1Y2l%2BBJalK43mp7amE%3D" alt="微信图片_20241015001305.png" width="90%"></p>

## 为什么选择 pnpm 作为包管理器？

1.  **Workspace 支持**

*   `原生支持 Monorepo`

pnpm 原生支持 Monorepo 项目，通过 `pnpm-workspace.yaml` 文件，你可以轻松地配置多个子项目。pnpm 的 workspace 功能使得在多个子项目之间共享依赖变得更加容易，并且可以自动处理依赖的安装和更新。

*   `依赖共享`

在 Monorepo 项目中，pnpm 的 workspace 功能允许你在多个子项目之间共享依赖。例如，你可以使用 `workspace:*` 来引用本地 workspace 中的包，而不需要发布到 npm 仓库。

2.  **性能优势**

*   `快速安装`

pnpm 使用了一种称为“内容寻址存储”（Content-Addressable Storage）的技术，这意味着相同的依赖包只会被下载和存储一次。这大大减少了安装时间，尤其是在大型 Monorepo 项目中，多个子项目共享相同的依赖时，pnpm 的性能优势尤为明显。

*   `节省磁盘空间`

由于 pnpm 只存储每个依赖包的一个副本，因此它可以显著减少磁盘空间的占用。这对于 Monorepo 项目尤为重要，因为多个子项目可能会共享大量的依赖包。

3.  **依赖管理**

*   `严格的依赖解析`

pnpm 使用了一个严格的依赖解析算法，确保每个包的依赖版本都是一致的。这减少了由于依赖版本不一致而导致的潜在问题，特别是在 Monorepo 项目中，多个子项目共享相同的依赖时。

*   `扁平化的 node_modules 结构`

pnpm 的 `node_modules` 结构是扁平化的，这意味着每个包都有自己的依赖目录，而不是将所有依赖放在根目录下。这种结构减少了依赖冲突的可能性，并且使得依赖关系更加清晰。

4.  **安全性**

*   `避免幽灵依赖`

pnpm 的 `node_modules` 结构避免了“幽灵依赖”（Phantom Dependency）问题。幽灵依赖是指在 `node_modules` 中意外地引入了未在 `package.json` 中声明的依赖包。pnpm 通过严格的依赖解析和扁平化的 `node_modules` 结构，减少了这种问题的发生。

5.  **灵活性**

*   `支持多种包管理方式`
    pnpm 支持多种包管理方式，包括 npm、yarn 等。你可以轻松地将现有的项目迁移到 pnpm，而不需要对项目结构进行大的改动。

## 初始化项目

1.  创建项目目录

首先新建一个文件夹，名为 `vue3-pnpm-monorepo`

进入 `vue3-pnpm-monorepo` 文件夹，初始化一个默认的 `package.json` 文件，执行命令：

```js
mkdir vue3-pnpm-monorepo
cd vue3-pnpm-monorepo

pnpm init
```

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/6fad8986d552435f9f9e8ab9b1af10b6~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=TUW%2BNkNx8wcLrR%2BRzA%2FpUCgSn5U%3D" alt="微信图片_20241015002127.png" width="90%"></p>

2. 配置 `pnpm workspace`

为了在 Monorepo 中管理多个项目，我们需要配置 pnpm workspace。在项目根目录下创建一个 `pnpm-workspace.yaml` 文件，并添加以下内容：

```js
packages:
  - 'packages/*'
```

这表示所有位于 `packages` 目录下的子目录都将被视为独立的包（项目）。**这个文件可以帮助我们在安装公共依赖的情况下，也将 `packages` 下的项目所需要的依赖也同时进行安装。**

3.  创建子项目目录

```js
mkdir packages
```

4.  初始化三个 `vue3 + ts` 的项目进行演示

```js
cd packages
npm init vite vue-demo1
npm init vite vue-demo2
npm init vite vue-demo3
```

当前项目结构如下：

```js
├── packages
|  ├── vue-demo1
|  ├── vue-demo2
|  └── vue-demo3
├── package.json
```

项目内部结构如下：

```js
├── packages
|  ├── vue-demo1
|  |  ├── .vscode
|  |  ├── public
|  |  ├── src
|  |  ├── .gitignore
|  |  ├── index.html
|  |  ├── package.json
|  |  ├── README.md
|  |  ├── tsconfig.json
|  |  ├── tsconfig.node.json
|  |  └── vite.config.ts
|  ├── vue-demo2
|  └── vue-demo3
├── package.json
```

每个子项目的 `package.json` 内容应该如下：

```js
{
  "name": "vue-demo1",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc -b && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.5.10"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.1.4",
    "typescript": "^5.5.3",
    "vite": "^5.4.8",
    "vue-tsc": "^2.1.6"
  }
}

```

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/582511c5db7d47bd8f50f0e6d1137e11~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=hatVZEzsFaGAaL5DJDzVA%2FYGdjc%3D" alt="微信图片_20241015003427.png" width="90%"></p>

经过分析不难发现，目前这三个项目是完全一样的，需要的依赖也是完全一样的，所以这些依赖项就可以直接抽离出来，变成公共的依赖项，添加上版本号，另外调试的话也不需要在这里进行调试，也直接删掉，稍加修改这个文件，最后如下：

vue-demo1:

```js
{
  "name": "vue-demo1",
  "private": true,
  "version": "0.0.0"
}
```

vue-demo2:

```js
{
  "name": "vue-demo2",
  "private": true,
  "version": "0.0.0"
}
```

vue-demo3:

```js
{
  "name": "vue-demo3",
  "private": true,
  "version": "0.0.0"
}
```

## 创建公共依赖配置

*   接下来就需要将三个公共的依赖项，进行配置到根目录，使用全局的依赖包提供这三个项目使用
*   在 根目录下的 `package.json` 新增之前抽离出来的公共配置项，都添加到公共的配置文件中
*   新增调试的命令，一般启动项目可以使用 `dev:项目名` 来进行分别启动项目，后面跟上需要启动的路径即可

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/b5a6e8bbdbce4c75864b106007ad53e9~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=GZnwgPpPDa8snQKfC7QDfYZQqG0%3D" alt="微信图片_20241015004633.png" width="90%"></p>

配置好之后，就可以在根目录执行：

```js
pnpm i
```

安装好了之后，我们就会发现，在项目的根目录中，有了 `node_modules` 文件夹。
接下来可以通过命令启动一下项目：

```js
pnpm run dev:vue-demo1
pnpm run dev:vue-demo2
pnpm run dev:vue-demo3
```

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/8eb60abdeca64e86aae6f231e60c42bb~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=yyavWsTo6U4S70Fd6MbReDGP%2BYA%3D" alt="微信图片_20241015005719.png" width="90%"></p>

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/c46cb1d384b44cad8d532a7e1fe27e38~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=gMZ5VouxmkVljzYKGB%2BsW6Zzv%2BU%3D" alt="微信图片_20241015005906.png" width="90%"></p>

## 局部安装依赖项

当然，我们也会遇到一种情况就是，我们的，某个子项目需要某个依赖，但是其他项目没有使用，所以这个时候最好就是单独为这个子项目安装依赖，比如说，我的 `vue-demo1` 的项目中需要安装 `element-plus`，而其它的两个项目是不需要的，那么这样的话，就可以将 `element-plus` 单独安装到 `vue-demo1` 的项目中，而另外两个项目是不需要的，所以就没必要安装到全局，直接安装到 `vue-demo1` 内部，安装的方式有两种：

1.  进入到指定目录去安装

可以直接进入到指定需要安装的目录进行安装，那么进入到 `packages/vue-demo1` 中，执行：

```js
cd packages/vue-demo1
pnpm i element-plus
```

2.  `--filter` 安装
    使用 `--filter` 修饰符可以实现在根目录指定某个目录进行安装，具体命令为：

```js
pnpm i element-plus --filter vue-demo1
```

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/4e0a4e1c33d04a6ab0c036198b571fcd~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=lvgT9c3pq%2Btc1xLq257fpqVYU6A%3D" alt="微信图片_20241015010645.png" width="90%"></p>

## 配置共享库

1.  packages 下面新建 shared-utils 目录，再新建 index.ts 文件，如图：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/cb2d182302124a8e874356bb21c0690e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=wWxIpDiOiTQjqP5pZ7qWrpihn8c%3D" alt="微信图片_20241015110353.png" width="90%"></p>

2.  shared-utils 下新建 package.json ，内容如下：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/5b562b3ca5ff4004b739384ca4c099d2~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=CPSxvu%2FYzXEU780o09G3wnW75%2Bg%3D" alt="微信图片_20241015110559.png" width="90%"></p>

3.  假设我们需要在 vue-demo1 中使用，则还需配置如下：

确保根目录的 `package.json` 中 `workspaces` 配置正确：

<img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/c09a532a339f4c948abf5fc0883f4654~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=qiZ2BOe7XfZlfTDcwtcAddI7OSw%3D" alt="微信图片_20241015110936.png" width="90%">

在 `packages/vue-demo1` 目录下，编辑 `package.json` 文件，添加对 `shared-utils` 的依赖：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/6815c7178304464dbf4157efa7a13593~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=jXvJV8Grw1tnHc%2BkyRy6piBpPtQ%3D" alt="微信图片_20241015111155.png" width="90%"></p>

5.  在根目录执行 `pnpm install`

6.  修改`vue-demo1`

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/b4742263b980429f89c434c7cccd14fb~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=RxgbLAacZwl5HfQJGBSiPKvSWAo%3D" alt="微信图片_20241015111555.png" width="90%"></p>

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/cbafab8bfea445759402b0f03ec360bf~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=A%2BQ%2BrJtQMr4deKB05DHl3oklPwA%3D" alt="微信图片_20241015111732.png" width="90%"></p>

9.  解决项目中的报错提示
    *   无法找到声明文件

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/331f064ce9f443499a2d5f5a85307f33~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=4xNOI%2F9GVdqVdk1qRI8X6PVXs90%3D" alt="微信图片_20241015112126.png" width="90%"></p>
这个错误是由于 TypeScript 无法找到 `shared-utils` 模块的类型声明文件。我们可以通过这种方式解决这个问题： 使用 `tsconfig.json` 配置， `vue-demo1` 项目的 `tsconfig.json` 中添加 `typeRoots` 配置：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/616ce466edcb489b9fc162324467434f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=pwj1oLrxvOpoqEXI4zBm9IibBBs%3D" alt="微信图片_20241015112543.png" width="90%"></p>

在 `src/types` 目录下创建 `shared-utils.d.ts` 文件：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/09dc874461ae4138b9fe30142e31af71~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=tfHDMD1g%2BvyVCo40Rknvk8LV9UE%3D" alt="微信图片_20241015112710.png" width="90%"></p>

*   shared-utils 放在 common 文件夹下面时 pnpm install 执行报错： In packages\vue-demo1: "shared-utils\@workspace:\*" is in the dependencies but no package named "shared-utils" is present in the workspace

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/45a7544bd40644aaa32e4d00bb6a9f8c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=MZZhgtREmjeRDXJ7UyIiWsWiP3E%3D" alt="微信图片_20241015135413.png" width="90%"></p>

这个时候需要在根目录的 pnpm-workspace.yaml 配置文件下增加目录，也就是 common 文件夹的目录，因为我们原本只写了 - 'packages/\*'，它的 workspace 只识别了一层，所以还需要配置 common 目录,让其识别 common 下面的目录文件，最终配置如下如下：

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/7041490fe8b14c22a6bbb0d2ba9c0af4~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=mpRVzUsQCGigSyvI74054TEfrQg%3D" alt="微信图片_20241015135654.png" width="90%"></p>

最后再次在根目录执行 `pnpm install` 即可

11. 根目录执行`pnpm run dev:vue-demo1`，运行子项目，这个时候就可以发现这个子项目下可以正常使用公共工具当中导出的变量和方法啦

<p align="center"><img src="https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/cc0d3d35451f4445a0cb88fb4006eaba~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oC75piv552h5LiN5aSf:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMjU3MTEzMTIzMTE1MDQ4MCJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1729602586&x-orig-sign=MFgtb63U87Z7hZF7lPqqIFMG9Wg%3D" alt="微信图片_20241015113301.png" width="90%"></p>

## 总结

Monorepo 的优势在于它能够在一个代码库中管理多个相关项目，从而提高代码复用性和开发效率。以下是 Monorepo 的一些主要优势：

1.  **代码复用**
    *   在 Monorepo 中，你可以轻松地在多个项目之间共享代码。通过创建共享库，你可以在不同的子项目中复用相同的代码，从而减少重复工作并提高代码的一致性。
2.  **统一依赖管理**
    *   使用 pnpm 的 workspace 功能，你可以在一个地方管理所有子项目的依赖。这不仅简化了依赖管理，还减少了重复安装相同依赖的情况，节省了磁盘空间。
3.  **简化开发流程**
    *   在 Monorepo 中，你可以使用统一的工具链和配置文件来管理多个项目。这简化了开发流程，使得开发者可以更专注于业务逻辑的实现，而不是配置和工具的调整。
4.  **更好的版本控制**
    *   在 Monorepo 中，所有项目都在同一个代码库中，这使得版本控制更加容易。你可以轻松地跟踪代码的变化，并在需要时回滚到之前的版本。
5.  **提高团队协作**
    *   Monorepo 使得团队成员可以更容易地共享代码和知识。通过在一个代码库中工作，团队成员可以更方便地协作，减少沟通成本，提高开发效率。

以上就是小编的本篇内容分享，感谢阅读！！！
