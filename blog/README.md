# [Exploit-me.com](https://exploit-me.com/)

## Project Tour

The monorepo is organized into five sub-projects: 

1. `hugo` - Static Site Generator. This is where the content lives
2. `design` - Theme & CSS
3. `components` - Angular Elements Web Components
4. `functions` - Firebase Cloud Functions Serverless Backend
5. `cypress` - End-to-End & Integration Specs



## Contributing

Edit and fix the site's content in `hugo/content/`. Feel free to submit PRs for small issues. For large issues or features, open an issue first. 

## Running the Site


First, install [Hugo](https://gohugo.io/getting-started/installing/).

```shell
git clone <fireship-repo>

npm install

npm run dev
```

Visit `localhost:1313` and you should be live. You do not need the web components for general content development, but they can be built with:

```shell
cd components && npm install
npm run build
```


## Contribute a Post

```shell
cd hugo
hugo new -k bundle lessons/angularfire-google-oauth
hugo new snippets/my-cool-snippet.md
```


## Web Component Development

Interactive features are built with Angular Elements web components in `components/`
