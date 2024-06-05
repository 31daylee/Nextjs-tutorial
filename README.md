## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

# Nextjs-tutorial

## Ch02

### The benefit of using CSS modules

- Provide a way to make CSS classes locally scoped to components by default, reducing the risk of styling conflicts

## Ch03

### The Image component

- The '<'Image'>' Component is an extension of the HTML <img> tag
- Preventing layout shift automatically when images are loading
- Resizing images to avoid shipping large images to devices with a smaller viewport
- Lazy loading images by default (images load as they enter the viewport)

## Ch04

### Creating the dashboard page

- For creating a new pages in Next.js : create a new route segment using a folder, and add a page file inside it.
  <br/>
  ex) If you want to create the new pages like http://localhost:3000/dashboard/customers
  then create a folder with the name "Customers" and add a page.tsx file inside it.
  <br/>

  > app/dashboard/customers/page.tsx

- Root layout vs Custom layout
  <br/>
  Root layout : /app/layout.tsx -> global
  <br/>
  Custom layout : /app/dashboard/layout.tsx -> only dashboard

## Ch05

### The Link component

- The <Link> Component is similar to
