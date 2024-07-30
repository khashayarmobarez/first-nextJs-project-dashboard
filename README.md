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