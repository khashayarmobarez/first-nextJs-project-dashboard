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