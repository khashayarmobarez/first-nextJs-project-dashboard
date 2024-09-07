# Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.



## chapter 1/3: Optimizing Fonts and Images:

### Adding a primary font:  
    `export const lusitana = Lusitana({
    weight: ['400', '700'],
    subsets: ['latin'],
    });`

### The <Image> component: 
    `<Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
    />`



## chapter 1/4: Optimizing Fonts and Images:

### Nested routing:
You can create separate UIs for each route using layout.tsx and page.tsx files.
To create a nested route, you can nest folders inside each other and add page.tsx files inside them. For example:
`/app/dashboard/page.tsx` is associated with the `/dashboard` path

page.tsx:
`
export default function Page() {
  return <p>Dashboard Page</p>;
}
`

Creating the dashboard layout
One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering

layout.tsx
import SideNav from '@/app/ui/dashboard/sidenav';
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}



## chapter 1/5: optimize navigation:


### The <Link> component:
 `<Link
    key={link.name}
    href={link.href}
    className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
    >
    <LinkIcon className="w-6" />
    <p className="hidden md:block">{link.name}</p>
    </Link>`


### Pattern, Showing active links:
A common UI pattern is to show an active link to indicate to the user what page they are currently on.
usePathname intentionally requires using a Client Component. It's important to note Client Components are not a de-optimization. They are an integral part of the Server Components architecture.

### usePathname() 
`
'use client';
import { usePathname } from 'next/navigation';

export default function NavLinks() {
  const pathname = usePathname();
  // ...
}
`

### conditional styling using clsx
`import clsx from 'clsx';


 className={clsx(
    'flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3',
    {
    'bg-sky-100 text-blue-600': pathname === link.href,
    },
)}`





## chapter 1/6: Setting Up Your Database(PostgreSQL):

### steps
1- push your repository to Github
2- Create a Vercel account
3- Connect and deploy your project
4- Create a Postgres database(serverless sql)
5- Seed your database /app/seed


## chapter 1/7: Fetching data:

### In Next.js, you can create API endpoints using Route Handlers.

### Using Server Components to fetch data
By default, Next.js applications use React Server Components.

Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.
As mentioned before, since Server Components execute on the server, you can query the database directly without an additional API layer.

### Using SQL

### What are request waterfalls?
A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.
This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

However, this behavior can also be unintentional and impact performance.

### Parallel data fetching
A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.
In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time. For example, in data.ts, we're using Promise.all() in the fetchCardData() function:



## chapter 1/8: Static and Dynamic Rendering:

### What is Static Rendering?

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or when revalidating data.
benefits:
  1-Faster Websites 
  2-Reduced Server Load 
  3-SEO
  
### What is Dynamic Rendering?

With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). There are a couple of benefits of dynamic rendering:
  1-Real-Time Data
  2-User-Specific Content  
  3-Request Time Information




## chapter 1/9: streaming:

### streaming
Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

### Streaming a whole page with loading.tsx

loading.tsx is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.
this loading page also applies to the child pages of the route that you added the file inside

### Fixing the loading skeleton bug with route groups(overview())

using (overview) makes the files to only apply for the folder(route) that they are apllied to and not their nests
Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path. So /dashboard/(overview)/page.tsx becomes /dashboard.

### Streaming a component

So far, you're streaming a whole page. But you can also be more granular and stream specific components using React Suspense.

  `
  <Suspense fallback={<RevenueChartSkeleton />}>
    <RevenueChart />
  </Suspense>
  `


## chapter 1/10: Partial Prerendering(experimental)

In this chapter, let's learn how to combine static rendering, dynamic rendering, and streaming in the same route with Partial Prerendering (PPR).

### What is Partial Prerendering?

Next.js 14 introduced an experimental version of Partial Prerendering – a new rendering model that allows you to combine the benefits of static and dynamic rendering in the same route.

### How does Partial Prerendering work? Implementing Partial Prerendering

Partial Prerendering uses React's Suspense. 

Implementing:

- Enable PPR for your Next.js app by adding the ppr option to your next.config.mjs file:

` @type {import('next').NextConfig} 
 
const nextConfig = {
  experimental: {
    ppr: 'incremental',
  },
};
 
export default nextConfig;`


The 'incremental' value allows you to adopt PPR for specific routes.

- Next, add the experimental_ppr segment config option to your dashboard layout:
`
export const experimental_ppr = true;
`

That's it. You may not see a difference in your application in development, but you should notice a performance improvement in production. Next.js will prerender the static parts of your route and defer the dynamic parts until the user requests them.
