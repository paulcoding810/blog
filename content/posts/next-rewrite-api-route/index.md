---
date: "2025-09-08T20:26:32+07:00"
draft: false
title: "Next.js API Route Rewrites"
summary: "Learn how to rewrite API routes in Next.js"
categories:
  - Code
tags:
  - nextjs
  - web
---

## Steps to Rewrite API Routes in Next.js

### 1. Create an API Handler

- For example, create the file `app/api/json/[slug]/route.ts`.
- You can use custom handlers in `NextResponse`.

```ts
// Example API handler
import { NextResponse } from "next/server";

export async function GET(request, { params }) {
  const { slug } = params;
  return NextResponse.json(
    { message: `Hello, ${slug}!` },
    {
      headers: {
        "Content-Type": "application/json",
        "Access-Control-Allow-Origin": "*",
      },
    }
  );
}
```

### 2. Update `next.config.mjs`

- As an example, rewrite `/hello` to `/api/json/hello` by adding the following configuration:

```js
const nextConfig = {
  async rewrites() {
    return [
      {
        source: "/:slug",
        destination: "/api/json/:slug",
      },
    ];
  },
};

export default nextConfig;
```

By following these steps, you can easily set up API route rewrites in your Next.js application.
