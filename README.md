

### Goals to learn after reading this article 

- [ ] Set up a Nuxt monorepo application
- [ ] Create a tailwind Nuxt Layer
- [ ] Use the tailwind Nuxt layer in the main application





#### Set up and requirements 


Before continuing with this article if you're following along please make sure that you have the following installed

- [ ] Nodejs
- [ ] pnpm
- [ ] vscode(or editor of choice)


### Step 1 Create the monorepo



- [ ] Create the base folder for our project


```sh
mkdir nuxt-monorepo-tailwind-layer

# Navigate to the folder

cd nuxt-monorepo-tailwind-layer

```


- [ ] Create package json 
```json
{
  "name": "nuxt-monorepo-tailwind-layer",
  "version": "1.0.0",
  "description": "Nuxt monorepo with tailwind layer",
  "scripts": {
    "packages": " pnpm --filter",
    "site": "pnpm packages @local-monorepo/site",
    "site:dev": "pnpm packages @local-monorepo/site dev",
    "site:build": "pnpm packages @local-monorepo/site build",
    "site:start": "pnpm packages @local-monorepo/site start",
    "dev": " pnpm -r dev",
    "build": " pnpm -r build",
    "start": "pnpm --stream -r run dev",
    "clean": "pnpm -r run clean"
  },
  "keywords": [],
  "author": "Ismael Garcia <leamsigc@leamsigc.com>",
  "license": "MIT"
}
```

In the `package.json` we call our main application site  and that is the nuxt application that will use the tailwind layer

- [ ] Create `pnpm-workspace.yaml`

``` sh
touch pnpm-workspace.yaml && mkdir packages
```


- [ ] Update the content of the `pnpm-workspace.yaml` file 

```yml
packages:
  - packages/* # You can change this to match your project structure or name where all the services or applications will go

```



- [ ] Create the layer and the basic application 

- Create the main application that is `@local-monorepo/site`

	`pnpm dlx nuxi@latest init ./packages/site `

- Update the  name in the `package/site/package.json ` to `@local-monorepo/site`



- [ ] Create the tailwind layer

- Create the basic layer `npx nuxi init --template layer ./packages/tailwind-layer `
- Update the `./packages/tailwind-layer/package.json` name to `@local-monorepo/tailwind-layer` 




##  Connect the tailwind-layer to the main application




- Add the tailwind-layer as dependence `"@local-monorepo/tailwind-layer": "workspace:^"`
- Add the layer to the `./packages/site/nuxt.config.ts`


```ts
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  compatibilityDate: '2024-11-01',
  devtools: { enabled: true },
  extends: ["@local-monorepo/tailwind-layer"],
})

```




## Test and create/use first component from the layer 



- In the tailwind-layer we will need to install the tailwind module 

Go to the base folder for the monorepo

Install all the dependencies 

```sh
pnpm i
```

start the dev server for the layer 

```sh

#nuxt-monorepo-tailwind-layer
pnpm packages tailwind-layer dev


```

- [ ] Install the tailwindcss module 
- [ ] And we can create our components that will be available in the site package



Example base on the default component from the Nuxt layer


We can use the `HelloWorld` component from our layer now.

`./packages/site/app.vue`

```vue
<template>
  <div>
    <NuxtRouteAnnouncer />
    <HelloWorld/>
    <NuxtWelcome />
  </div>
</template>

```

We can start the dev server for the site `pnpm site:dev`