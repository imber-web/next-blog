

> 请求接口多，不利于首屏渲染

Link（不需要路由系统）、Image标签、Head标签

### 代码拆分和预取

Next.js会自动进行代码拆分，因此每个页面只加载该页面所需的内容。这意味着当主页呈现时，最初不会提供其他页面的代码。

这确保了即使您有数百页，主页也能快速加载。

仅加载您请求的页面的代码也意味着页面被隔离。如果某个页面抛出错误，应用程序的其余部分仍然有效。

此外，在Next.js的生产版本中，每当[`Link`](https://nextjs.org/docs/api-reference/next/link)组件出现在浏览器的视口中时，Next.js会自动**预取**后台链接页面的代码。在您单击链接时，目标页面的代码将已加载到后台，页面过渡将接近即时！

### 摘要

Next.js通过代码拆分、客户端导航和预取（生产中）自动优化您的应用程序，以获得最佳性能。

您可以在[`pages`](https://nextjs.org/docs/basic-features/pages)下将路由创建为文件，并使用内置[`Link`](https://nextjs.org/docs/api-reference/next/link)组件。不需要路由库。

> **注意：**如果您需要链接到Next.js应用程序之外的*外部*页面，只需使用没有`Link`的`<a>`标签即可。
>
> 如果您需要添加属性，例如`className`，请将其添加到标签中，*而不是*`Link`标签中。[这里有一个例子](https://github.com/vercel/next-learn/blob/master/basics/snippets/link-classname-example.js)。

[CSS模块](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css)对组件级样式非常有用。但是，如果您希望**每个页面都**加载一些CSS，Next.js也支持它。

要加载[全局CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet)文件，请**创建一个名为[`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app)的文件**

此`App`组件是顶级组件，在所有不同页面上都很常见。例如，您可以在页面之间导航时使用此`App`组件来保持状态。

在Next.js中，您可以通过从[`pages/_app.js`](https://nextjs.org/docs/advanced-features/custom-app)导入[全局CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet)文件来添加它们。您**无法**在其他地方导入全局CSS。

[全局CSS](https://nextjs.org/docs/basic-features/built-in-css-support#adding-a-global-stylesheet)无法导入到`pages/_app.js`之外的原因是全局CSS会影响页面上的所有元素。

如果您要从主页导航到`/posts/first-post`页面，则主页的全局样式将无意中影响`/posts/first-post`。

默认情况下，Next.js预渲染每个页面。这意味着Next.js*提前为每个页面生成HTML*，而不是由客户端JavaScript完成。预渲染可以带来更好的性能和[搜索引擎优化](https://en.wikipedia.org/wiki/Search_engine_optimization)。

每个生成的HTML都与该页面所需的最小JavaScript代码相关联。当浏览器加载页面时，其JavaScript代码会运行，并使页面完全交互。（这个过程被称为**水合作用**。）

## getStaticProps

Essentially, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) allows you to tell Next.js: *“Hey, this page has some data dependencies — so when you pre-render this page at build time, make sure to resolve them first!”*

> **Note**: In development mode, [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) runs on each request instead.

## 什么时候适合客户端渲染？SSG/SSR?

## SWR

Next.js 背后的团队创建了一个名为[**SWR**](https://swr.vercel.app/)的用于数据获取的 React 钩子。如果您在客户端获取数据，我们强烈推荐它。它处理缓存、重新验证、焦点跟踪、间隔重新获取等。我们不会在这里详细介绍，但这里有一个示例用法