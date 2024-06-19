---
title: "Preview Mode for Static Generation"
excerpt: "Learn how to implement preview mode in Next.js to allow content editors to see changes before they go live. This guide covers the setup and benefits of using preview mode with static generation."
coverImage: "/assets/blog/preview/cover.jpg"
date: "2020-03-16T05:35:07.322Z"
author:
  name: Joe Haddad
  picture: "/assets/blog/authors/joe.jpeg"
ogImage:
  url: "/assets/blog/preview/cover.jpg"
---

Learn how to implement preview mode in Next.js to allow content editors to see changes before they go live. This guide covers the setup and benefits of using preview mode with static generation.

## What is Preview Mode?

Preview mode in Next.js allows you to bypass the static generation and fetch data on every request. This is useful for content editors who need to see changes in real-time before they are published. It enables a dynamic experience on a statically generated site, providing a way to preview drafts or unpublished content.

## Benefits of Preview Mode

1. **Real-time Updates**: Content editors can see changes immediately without waiting for a full site rebuild.
2. **Draft Previews**: Preview unpublished content or drafts, ensuring everything looks perfect before going live.
3. **Enhanced Workflow**: Improve the content editing workflow by integrating a seamless preview experience.

## Implementing Preview Mode in Next.js

Implementing preview mode in Next.js involves setting up API routes to enable and disable preview mode and modifying your data fetching logic to account for preview states.

### Step 1: Enabling Preview Mode

Create an API route to enable preview mode. This will set preview cookies.

```javascript
// pages/api/preview.js

export default function handler(req, res) {
  // Enable Preview Mode by setting the cookies
  res.setPreviewData({});
  // Redirect to the home page or any other page
  res.writeHead(307, { Location: '/' });
  res.end();
}
```

### Step 2: Disabling Preview Mode

Create an API route to exit preview mode.

```javascript
// pages/api/exit-preview.js

export default function handler(req, res) {
  // Clear Preview Mode cookies
  res.clearPreviewData();
  // Redirect to the home page or any other page
  res.writeHead(307, { Location: '/' });
  res.end();
}
```

### Step 3: Fetching Data in Preview Mode

Modify your data fetching functions to check for preview mode and fetch draft content if needed.

```javascript
// pages/posts/[id].js

import { useRouter } from 'next/router';
import { getAllPostIds, getPostData, getPreviewPostData } from '../../lib/posts';

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

export async function getStaticProps({ params, preview = false }) {
  const postData = preview ? await getPreviewPostData(params.id) : await getPostData(params.id);
  return {
    props: {
      postData
    }
  };
}
```

In this example, the `getStaticProps` function checks if the preview mode is enabled and fetches the appropriate data accordingly.

## Conclusion

Preview mode in Next.js is a powerful feature that enhances the static generation workflow. It allows content editors to preview changes in real-time, improving the accuracy and efficiency of content updates. By integrating preview mode, you can create a more dynamic and responsive content editing experience, ensuring that your static site remains both performant and flexible. 

Implementing preview mode requires a few changes to your API routes and data fetching logic, but the benefits it brings to the content workflow are well worth the effort. Leverage preview mode to streamline your content editing process and ensure that every change is perfect before it goes live.