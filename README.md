# Ecommerce

Welcome to our ecommerce project! This README provides all the necessary information to get started, including links to our Jira board and Figma designs, as well as instructions for running the project locally.

## Running the Project Locally

To run the project locally, follow these steps:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/BuilderIO/unified-demo
   cd ecommerce

## Getting Started

First, run the development server:

```bash
npm run install
npm run dev

```
## Additional Information

- Refer to the `.env.example` file to understand the required environment variables format.
- Ensure that your local environment is configured correctly to match the hostname-based environment variable setup in `middleware.ts`.

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.



## Repository Structure

This ecommerce project is built using Next.js and integrates with Builder.io for content management. Here's an overview of the key directories and files:

1. **`app/`**: Contains the main application pages and layouts.
   - `layout.tsx`: The root layout component.
   - `page.tsx`: The homepage component.
   - Various subdirectories for different routes (e.g., `category/`, `product/`, etc.).

2. **`lib/`**: Utility functions and helpers.
   - `utils.ts`: Contains utility functions like `cn()` for class name merging.

3. **`public/`**: Static assets like images and fonts.

4. **`components/`**: Houses reusable React components.

5. **`component/mappings/`**: This directory contains mapping files for Builder.io components. These mappings define how Figma designs are translated into React components.

   - `generic.mapper.tsx`: Defines generic mappings for Figma elements.

```tsx
import { figmaMapping } from "@builder.io/dev-tools/figma";

figmaMapping({
  genericMapper(figma) {
    if (figma.$type === "FRAME" && figma.$name === "Section") {
      // If we have a FRAME node named "Section" then we will render it as a <section> element.
      return <section>{figma.$children}</section>;
    }

    // For other nodes, do not apply the generic
    // mapper and keep the default rendering behavior
    return undefined;
  },
});
```


   - `Footer.mapper.tsx`: Maps the Footer component from Figma to React.

```tsx
import { figmaMapping } from "@builder.io/dev-tools/figma";
import Footer from "@/components/Layout/Footer";

figmaMapping({
  componentKey: "df8ccf9e902602249fa68217953daab094570bba",
  mapper(figma) {
    return <Footer></Footer>;
  },
});
```


   - `Header.mapper.tsx`: Maps the Header component from Figma to React.

```tsx
import { figmaMapping } from "@builder.io/dev-tools/figma";
import { Header } from "@/components/Layout/Header";

figmaMapping({
  componentKey: "bf08eba01818598edd0d7ca65a48372fa4395184",
  mapper(figma) {
    return <Header></Header>;
  },
});
```


   - `Hero.mapper.tsx`: Maps various Hero components from Figma to React.

```tsx
import { figmaMapping } from "@builder.io/dev-tools/figma";
import ImageHero from "@/components/Hero/ImageHero";

figmaMapping({
  componentKey: "4bd6da0f53b73a462b070b55dd055ce6a4cb3eca",
  mapper(figma) {
    return (
      <ImageHero
        title={
          figma.Title ?? figma.$children[1]?.$children[0]?.$textContent ?? ""
        }
        subTitle="Discover our collection"
        buttonText={
          figma.buttonText ??
          figma.$findOneByName("Shop Now")?.$textContent ??
          ""
        }
        buttonLink="#"
        backgroundImage={figma.$children[0]?.$imageUrl ?? ""}
        alignment="center"
        makeFullBleed={false}
      />
    );
  },
});
```


   - `design-tokens.mapper.tsx`: Defines mappings for design tokens (colors, etc.).

```tsx
import { figmaMapping } from "@builder.io/dev-tools/figma";

figmaMapping({
  designTokenMapper(token) {
    if (token === "black") {
      return "var(--company-black, #000)";
    }

    // For anything keep the same
    return undefined;
  },
});
```


6. **Configuration Files**:
   - `next.config.mjs`: Next.js configuration file.
   - `tailwind.config.ts`: Tailwind CSS configuration.
   - `tsconfig.json`: TypeScript configuration.
   - `package.json`: Project dependencies and scripts.

7. **`builder-registry.ts`**: Registers custom components and design tokens with Builder.io.

```tsx
"use client";
import "@builder.io/widgets";
import { builder, Builder, withChildren } from "@builder.io/react";
import { Button } from "./components/ui/button";
import Counter from "./components/Counter/Counter";
import HeroWithChildren from "./components/Hero/HeroWithChildren";
import IconCard from "./components/Card/IconCard";
import ImageHero from "./components/Hero/ImageHero";
import SplitHero from "./components/Hero/SplitHero";
import TextHero from "./components/Hero/TextHero";

builder.init(process.env.NEXT_PUBLIC_BUILDER_API_KEY!);

Builder.register("editor.settings", {
  styleStrictMode: false,
  allowOverridingTokens: true, // optional
  models: ["page"],
  designTokens: {
    colors: [
      { name: "Primary", value: "var(--color-primary, #000000)" },
      { name: "Secondary", value: "var(--color-secondary, #ffffff)" },
      { name: "Deconstructive", value: "var(--color-deconstructive, #18B4F4)" },
      { name: "Muted", value: "var(--color-muted, #C8E2EE)" },
      { name: "Accent", value: "var(--color-accent, #F35959)" },
      { name: "Energetic", value: "var(--color-energetic, #A97FF2)" },
      { name: "Background", value: "var(--color-background, #ffffff)" },
      { name: "Text", value: "var(--color-primary, #000000)" },
      { name: "Text Muted", value: "var(--color-muted, #e2e8f0)" },
      {
        name: "Background Light",
        value: "var(--color-background-light, #FAFAFA)",
      },
    ],
    spacing: [
      { name: "Large", value: "var(--space-large, 20px)" },
      { name: "Small", value: "var(--space-small, 10px)" },
      { name: "Tiny", value: "5px" },
    ],
    fontFamily: [{ name: "Primary", value: "var(--primary-font, Poppins)" }],
    fontSize: [
      { name: "Small", value: "var(--font-size-small, 12px)" },
      { name: "Medium", value: "var(--font-size-medium, 14px)" },
      { name: "Large", value: "var(--font-size-large, 16px)" },
    ],
    fontWeight: [
      { name: "Light", value: "var(--font-weight-light, 200)" },
      { name: "Normal", value: "var(--font-weight-regular, 400)" },
      { name: "Medium", value: "var(--font-weight-medium, 600)" },
      { name: "Bold", value: "var(--font-weight-bold, 800)" },
    ],
    letterSpacing: [
      { name: "Tight", value: "var(--letter-spacing-tight, -0.02em)" },
      { name: "Normal", value: "var(--letter-spacing-normal, 0em)" },
      { name: "Relaxed", value: "var(--letter-spacing-wide, 0.02em)" },
      { name: "Loose", value: "var(--letter-spacing-wide, 0.04em)" },
    ],
    lineHeight: [
      { name: "None", value: "var(--line-height-none, 1)" },
      { name: "Tight", value: "var(--line-height-tight, 1.2)" },
      { name: "Normal", value: "var(--line-height-normal, 1.5)" },
      { name: "Relaxed", value: "var(--line-height-relaxed, 1.8)" },
      { name: "Loose", value: "var(--line-height-loose, 2)" },
    ],
  },
});
Builder.register("insertMenu", {
  name: "Heros",
  items: [
    { name: "TextHero" },
    { name: "ImageHero" },
    { name: "SplitHero" },
    { name: "HeroWithChildren" },
  ],
  // priority: 2,
});
Builder.register("insertMenu", {
  name: "Cards",
  items: [{ name: "IconCard" }, { name: "ProductCard" }],
  // priority: 3,
});
if (Builder.isBrowser) {
  if (builder.editingModel === "homepage") {
    Builder.register("insertMenu", {
      name: "Layout",
      items: [
        { name: "Columns" },
        { name: "Builder:Carousel" },
        { name: "Collection" },
      ],
    });
  }
}
Builder.register("insertMenu", {
  name: "Popups",
  items: [{ name: "UpsellPopup" }],
});

Builder.registerComponent(Counter, {
  name: "Counter",
  image: "https://cdn.builder.io/api/v1/image/assets%2Fa87584e551b6472fa0f0a2eb10f2c0ff%2F000c4b516154412498592db34d340789",
  inputs: [
    {
      name: "initialCount",
      type: "number",
    },
  ],
});
```
