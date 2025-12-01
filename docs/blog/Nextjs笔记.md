---
title: Nextjs笔记
createTime: 2025/11/24 16:45:42
permalink: /blog/ixxa4sj1/
tags:
  - nodejs
  - nextjs
---

## 动态路由 + 静态生成

基于参数的动态路由，静态生成是可以用的，比如`pages/blog/[id].js`,但需要配合 getStaticPaths 和 getStaticProps 来告诉
Next.js 哪些页面需要在构建时生成。

```js
import {getAllBlogIds, getBlogById} from '../../lib/blog';

export async function getStaticPaths() {
    // 获取所有博客 ID，用于生成静态页面
    const paths = getAllBlogIds().map(id => ({
        params: {id: id.toString()}, // params 必须是字符串
    }));

    return {paths, fallback: false}; // fallback: false 表示其他 id 会 404
}

export async function getStaticProps({params}) {
    const blog = getBlogById(params.id);

    return {
        props: {
            blog,
        },
    };
}

export default function BlogPage({blog}) {
    return (
        <div>
            <h1>{blog.title}</h1>
            <p>{blog.content}</p>
        </div>
    );
}
```

## Middleware 运行在 Edge runtime 而不是 Node runtime

Middleware 在 Next.js 中运行于 **Edge Runtime**，而不是传统的 Node.js Runtime。

由于 Edge Runtime 与 Node.js 不完全兼容，**Node.js 原生模块**（如 `fs`、`path`、`crypto`（部分）等） 或者依赖于此的第三方库 **无法在 Middleware 中使用**。

可以使用 Web API 或支持 Edge 的第三方库（如 `Web Crypto API` 替代 Node `crypto`）。

Middleware 是全局唯一的，**不像 Express 可以使用多个中间件链**。