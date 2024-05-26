# @thaitype/vitepress-typed-navbar

Vitepress Utils Library for Wrapping type-safe Navbar for support localization (i18n)

## Installation

```bash
npm install --save-dev @thaitype/vitepress-typed-navbar
```

## Usage

Setup the base sidebar for english and then using `clone` to create a new sidebar for another language.

```ts
import { Sidebar } from "@thaitype/vitepress-typed-navbar";
const baseSidebar = new Sidebar()
      .addGroup("/", { text: "Start Reading" })
      .addGroup("/loop", { text: "Loop" })
      .addGroup("/loop/mapped-types", { text: "Mapped Types" })
      .add("/loop/mapped-types", "intro", { text: "Introduction" });

// English sidebar
const enSidebar = baseSidebar.clone().toSidebarItems();

// Thai sidebar
const thSidebar = baseSidebar
  .clone()
  .override("/loop/mapped-types/intro", { text: "ชนิดข้อมูลแบบ Mapped" })
  .toSidebarItems("/th");
```

Example output for Thai sidebar, you can see the prefix `/th` in the link

```ts
[
  {
    key: "/",
    text: "Start Reading",
  },
  {
    key: "/loop",
    text: "Loop",
    items: [
      {
        key: "/loop/mapped-types",
        text: "Mapped Types",
        items: [
          { 
            key: "/loop/mapped-types/intro", 
            text: "ชนิดข้อมูลแบบ Mapped", 
            link: "/th/loop/mapped-types/intro"
          }
        ],
      },
    ],
  },
]
```

Then you can use it in the Vitepress config

```ts
import { defineConfig } from "vitepress";


export const en = defineConfig({
  lang: "en-US",
  themeConfig: {
   
    sidebar: enSidebar,

    // ... other options
  },
});

const th = defineConfig({
  lang: "th-TH",
  themeConfig: {
   
    sidebar: thSidebar,

    // ... other options
  },
});


// Main config `./vitepress/config.ts`
import { defineConfig } from 'vitepress'

export default defineConfig({
  ...shared,
  locales: {
    root: { label: 'English', ...en },
    th: { label: 'ภาษาไทย', ...th },
  }
})
```

You can see the full example in my TypeScript book:
https://github.com/mildronize/type-safe-design-pattern/blob/main/docs/.vitepress/shared.ts