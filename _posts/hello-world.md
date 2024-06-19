---
title: "Learn How to Pre-render Pages Using Static Generation with Next.js"
excerpt: "Static generation with Next.js allows you to pre-render pages at build time, enhancing performance and SEO. This guide walks you through the process and benefits of using static generation."
coverImage: "/assets/blog/hello-world/cover.jpg"
date: "2020-03-16T05:35:07.322Z"
author:
  name: Tim Neutkens
  picture: "/assets/blog/authors/tim.jpeg"
ogImage:
  url: "/assets/blog/hello-world/cover.jpg"
---

Static generation with Next.js allows you to pre-render pages at build time, enhancing performance and SEO. This guide walks you through the process and benefits of using static generation.

## What is Static Generation?

Static generation refers to the process of generating HTML pages at build time. This approach pre-renders pages and serves them as static files, resulting in faster load times and improved SEO. Unlike server-side rendering, where pages are rendered on each request, static generation builds the pages once and reuses them for every subsequent request.

## Benefits of Static Generation

1. **Performance**: Pre-rendered pages load quickly as they are served directly from a content delivery network (CDN).
2. **SEO**: Pages are fully rendered at build time, making it easier for search engines to index them.
3. **Scalability**: Serving static files can handle high traffic without significant server load.

## Implementing Static Generation in Next.js

Next.js makes it straightforward to implement static generation. You can create static pages using the `getStaticProps` and `getStaticPaths` functions.

### Example: Creating a Blog with Static Generation

Here's an example of how to create a blog with static generation using Next.js.

#### Step 1: Fetching Data at Build Time

Use `getStaticProps` to fetch data at build time.

```javascript
// pages/posts/[id].js

import { getAllPostIds, getPostData } from '../../lib/posts';

export default function Post({ postData }) {
  return (
    <article>
      <h1>{postData.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
    </article>
  );
}

export async function getStaticPaths() {
  const paths = getAllPostIds();
  return {
    paths,
    fallback: false
  };
}

export async function getStaticProps({ params }) {
  const postData = await getPostData(params.id);
  return {
    props: {
      postData
    }
  };
}
```

In this example, `getStaticPaths` generates the paths for all posts at build time, and `getStaticProps` fetches the data needed for each post.

#### Step 2: Generating Paths

The `getStaticPaths` function generates the paths for each post based on the IDs fetched from the data source.

```javascript
// lib/posts.js

export function getAllPostIds() {
  const fileNames = fs.readdirSync(postsDirectory);
  return fileNames.map(fileName => {
    return {
      params: {
        id: fileName.replace(/\.md$/, '')
      }
    };
  });
}
```

#### Step 3: Fetching Post Data

The `getStaticProps` function fetches the data for each post at build time.

```javascript
// lib/posts.js

export async function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`);
  const fileContents = fs.readFileSync(fullPath, 'utf8');

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents);

  // Combine the data with the id
  return {
    id,
    ...matterResult.data,
    contentHtml: processedContent
  };
}
```

## Conclusion

Static generation with Next.js offers a powerful way to enhance the performance and SEO of your web applications. By pre-rendering pages at build time, you can deliver a faster and more reliable user experience. Whether you're building a blog, an e-commerce site, or a corporate website, static generation can help you achieve your performance and scalability goals.

Understanding and implementing static generation can significantly improve the development workflow and user experience. Leverage Next.js to take full advantage of static generation and build high-performance web applications.