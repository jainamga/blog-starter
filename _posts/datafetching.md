---
title: "Static Generation with Data Fetching in Next.js"
excerpt: "Learn how to use static generation with data fetching in Next.js to create fast, SEO-friendly pages. This article covers the basics of data fetching methods in static generation."
coverImage: "/assets/blog/static-generation-data-fetching/cover.jpg"
date: "2020-06-15T11:00:07.322Z"
author:
name: Jane Smith
picture: "/assets/blog/authors/jane.jpeg"
ogImage:
url: "/assets/blog/static-generation-data-fetching/cover.jpg"
Learn how to use static generation with data fetching in Next.js to create fast, SEO-friendly pages. This article covers the basics of data fetching methods in static generation.
---

What is Static Generation with Data Fetching?
Static generation with data fetching involves pre-rendering pages at build time with data fetched from an external source. This approach allows you to create static pages that include dynamic data, combining the best of both static and dynamic worlds.

Benefits of Static Generation with Data Fetching
Performance: Pre-rendered pages load quickly as they are served from a CDN.
SEO: Fully rendered pages improve search engine indexing.
Dynamic Content: Fetch and include dynamic data at build time.
Implementing Static Generation with Data Fetching in Next.js
Next.js provides several data fetching methods for static generation, including getStaticProps and getStaticPaths.

Example: Building a Product Page with Static Generation
Here's an example of how to use static generation with data fetching to build a product page:

javascript
Copy code
// pages/products/[id].js

import { getAllProductIds, getProductData } from '../../lib/products';

export default function Product({ productData }) {
  return (
    <article>
      <h1>{productData.name}</h1>
      <p>{productData.description}</p>
      <p>${productData.price}</p>
    </article>
  );
}

export async function getStaticPaths() {
  const paths = getAllProductIds();
  return {
    paths,
    fallback: false
  };
}

export async function getStaticProps({ params }) {
  const productData = await getProductData(params.id);
  return {
    props: {
      productData
    }
  };
}
In this example, getStaticPaths generates the paths for all products at build time, and getStaticProps fetches the data needed for each product.

Conclusion
Static generation with data fetching in Next.js enables you to build fast, SEO-friendly pages that include dynamic content. By leveraging data fetching methods like getStaticProps and getStaticPaths, you can create a highly performant and scalable web application. Use static generation with data fetching to take full advantage of static site benefits while incorporating dynamic data.

