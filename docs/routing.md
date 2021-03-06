# Routing

Gridsome uses [vue-router] (https://router.vuejs.org/) for routing.


## Page routing

All `.vue` files in the `src/pages` directory will become the pages for your
website. The page URL is generated based on the location and name of the file.
The path will be lowercased and slugified automatically. So if have a file with
the name `AboutUs.vue`, it will get the slug `about-us`. Files named `Index.vue`
are treated like `index.html` files and will not get a slug. As an example:
`src/pages/features/Index.vue` will become `/features` and
`src/pages/features/Awesome.vue` will become `/features/awesome`.


## Routing for data source plugins

Data sources adds routing automatically with settings. These could be different for each plugin so check the plugin documentation how to use route.

```js
module.exports = {
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
        path: 'blog/**/*.md',
        typeName: 'BlogPost',
        route: '/blog/:year/:month/:day/:slug'
      }
    }
  ]
}
```

## Default route params
The default route params are `:year`, `:month`, `:day` and `:slug`. 


## Custom route params
There is possible to use fields coming from a GraphQL node type as route. Field values are slugified, but the original value will be available as **{fieldname}_raw**. Only root level primitive fields will be available as params.


## Routing for custom data sources
When you add a custom data source you need to use the `route` option inside `addContentType()` **OR** use `path` option inside `addNode()`. `route` will be used for all posts and `path` will be set per post. It's only possible to use one of them. If both are used `route` will be prioritzed.

Learn more about [custom data sources here](/docs/data-custom)

```js
const axios = require('axios')

module.exports = function (api) {
  api.loadSource(async store => {
    const { data } = await axios.get('https://api.example.com/posts')

    const contentType = store.addContentType({
      typeName: 'BlogPosts'
      route: 'blog/:slug'  // add this...
    })

    for (const item of data) {
      contentType.addNode({
        id: item.id,
        title: item.title
        path: 'blog/:slug' //... or this
      })
    }
  })
}
```


