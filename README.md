# Nextjs-tutorial
![Screenshot_1](https://github.com/31daylee/Nextjs-tutorial/assets/136422529/59118c49-2d12-4612-b239-ae57f7231876)

## Ch02

### The benefit of using CSS modules

- Provide a way to make CSS classes locally scoped to components by default, reducing the risk of styling conflicts

## Ch03

### The Image component

- The `<Image>` Component is an extension of the HTML <img> tag
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

- The `<Link href="...">`Component is similar to `<a href="...">`
- Active links
  <br />
  #### Checking the user's current path: `usePathname()`
- You can use the hook called `usePathname()`

  1. Add React's "use client" directive to the top of the file
  2. Import usePathname() from next/navigation
  3. Assign the path to a variable called pathname inside your `<NavLinks />` component
  4. Add styles using `clsx`

## Ch06

### Vercel Postgres

#### install

`npm i @vercel/postgres`

#### Database seed

1. Add "seed": "node -r dotenv/config ./scripts/seed.js" for "scripts" in package.json file
2. `npm run seed`

## Ch07

### Waterfalls

- A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests
- In the case of data fetching, each request can only begin once the previous request has returned data

### Parallel data fetching

- A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel
  ex) await Promise.all()

## Ch08

### Static Rendering

- With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during revalidation
- Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page

### Dynamic Rendering

- With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page)

### unstable_noStore

- Next.js API
- You can use this API inside your Server Components or data fetching functions to opt out of static rendering
  ex) import { unstable_noStore as noStore } from 'next/cache';
  `noStore()`

### Slow Data Fetch

Added an artificial 3-second delay to simulate a slow data fetch
`await new Promise((resolve) => setTimeout(resolve, 3000))`
With dynamic rendering, your application is only as fast as your slowest data fetch

## Ch09

### Streaming

- Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready
- How to implement it?
  1. At the page level, with the `loading.tsx` file.
     > /app/dashboard/loading.tsx
     1. `return <div>Loading...</div>`
     2. `return <DashboardSkeleton />;`
  2. For specific components, with `<Suspense>`

### () folder

- It won't be included in the URL path.
  ex) `/dashboard/(overview)/page.tsx` become `/dashboard`

## Ch11

### Search

- `useSearchParams` : Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`
- `usePathname` : Lets you read the current URL's pathname
- `useRouter` : Enables navigation between routes within client components programmatically
- `URLSearchParams` : a Web API that provides utility methods for manipulating the URL query parameters
  <br/>
  Instead of creating a complex string literal, you can use it to get the params string like `?page=1&query=a`
  <br/>

#### `defaultValue` vs `value` / Controlled vs. Uncontrolled

- If you're using state to manage the value of an input, you'd use the value attribute to make it a controlled component. This means React would manage the input's state.

However, since you're not using state, you can use defaultValue. This means the native input will manage its own state. This is okay since you're saving the search query to the URL instead of state.

#### `useSearchParams()` hook vs. the `searchParams` prop

- `<Search>`is a Client Component, so you used the `useSearchParams()` hook to access the params from the client.
- `<Table>` is a Server Component that fetches its own data, so you can pass the `searchParams` prop from the page to the component.

### Installation of Debouncing

> npm i use-debounce

#### Debouncing?

- a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing

#### How?

- In your `<Search>` Component, import a function called `useDebouncedCallback`

## Ch12

### Using forms with Server Actions

- In React, you can use the action attribute in the `<form>` element to invoke actions. The action will automatically receive the native FormData object, containing the captured data
- Revalidate the associated cache using APIs : `revalidatePath`, `revalidateTag`

### Making Invoice

1. Create a form
   > `/dashboard/invoices/create/page.tsx`

<br/>

2. Create a Server Action and invoke it from the form

- Use server

  > `use server`

  - By adding the `'use server'`, you mark all the exported functions within the file as server functions. These server functions can then be imported into Client and Server components, making them extremely versatile

- Server actions

  > `use server export async function createInvoice(formData: FormData) {}`

<br/>

3. Inside your Server Action, extract the data from the `formData` object.

- `.get(name)` : To extract the values of `formData`
- `Object.fromEntries(formData.entries())` : To extract many fields of form

<br/>

4. Validate and prepare the data

- `Zod` : a TypeScript-first validation library
- Create new date ("YYYY-MM-DD")
  `const date = new Data().toISOString().split('T')[0]`

<br/>

5. Inserting the data into your database
6. Revalidate and redirect

- `revalidatePath` : To clear the cache and trigger a new request to the server.
- `redirect` from 'next/navigation': redirect function
  <br/>

### Update Invoices

1. Create a Dynamic Route Segment with the invoice `id`

- You can create dynamic route segments by wrapping a folder's name in square brackets. For example, `[id]`, `[post]` or `[slug]`
  > invoices/[id]/edit/page.tsx

2. Read the invoice `id` from page `params`
3. Fetch the specific invoice
4. Pass the `id` to the Server Action

### Deleting an invoice

1. Wrap the delete button in a `<form>` element and pass the `id` to the Server Action using `bind`
   <br/>
   `export function DeleteInvoice({ id }: { id: string }) { const deleteInvoiceWithId = deleteInvoice.bind(null, id);`

## Ch14

### Using the ESLint accessibility plugin

1. Add `next lint` as a script in `package.json` file.
   > "lint": "next lint"
2. Run npm run lint in your terminal
   > npm run lint

## Ch15

### NextAuth.js

1. Install
   > npm install next-auth@beta
2. Generate a secret key
   > openssl rand -base64 32

## Ch16

### Metadata

- In web development, metadata provides additional details about a webpage. Metadata is not visible to the users visiting the page. Instead, it works behind the scenes, embedded within the page's HTML, usually within the `<head>` element
  1. Title Metadata : Responsible for the title of a webpage that is displayed on the browser tab
  2. Description Metadata : This metadata provides a brief overview of the webpage content and is often displayed in search engine results
  3. Keyword Metadata : This metadata includes the keywords related to the webpage content, helping search engines index the page
  4. Open Graph Metadata : This metadata enhances the way a webpage is represented when shared on social media platforms, providing information such as the title, description, and preview image
  5. Favicon Metadata : This metadata links the favicon (a small icon) to the webpage, displayed in the browser's address bar or tab
