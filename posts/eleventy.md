---
title: 11ty basic features.
description: 'My thoughts on 11ty'
date: 2021-07-01
tags:
  - SSG
  - frontend
  - eleventy
layout: layouts/post.njk
---

I was looking into several SSG alternatives and and came across <a href="https://www.11ty.dev/">11ty</a>, a static site generator written in Node.js. Here's the reason why I loved it:

### Minimum requirement, highly configurable

11ty is great because of its zero boilerplate nature. There are many SSG that requires you to learn certain programming language or frameworks before you could start working with it. If you don't prefer to start off with this pre-requisite, this is perfect for you. 
As long as you understand the gist of static sites (HTML, CSS, some JS), then the only files you really need to get a static site build on 11ty is:

- an `index` file, which can be written in any templating language (`.md`, `.html`, `.njk` etc.), and</li>
- an 11ty config file `.eleventy.js`, which is expected in the root directory of your project.

The `.eleventy.js` config file is where you can customize your input and output directory, configure your plugins and filters, and add other customization you need to your 11ty site.

```js
module.exports = function(eleventyConfig) {
	// Add plugins
	eleventyConfig.addPlugin(pluginRss);

	// Add filters
	eleventyConfig.addFilter('htmlDateString', (dateObj) => {
		return DateTime.fromJSDate(dateObj, {zone: 'utc'}).toFormat('yyyy-LL-dd');
	});

	return {
		// Customizing input and output directory
		dir: {
			input: "src",
			output: "public"
		}
	};
};
```

Unlike some static site generator like [Gatsby.js](https://www.gatsbyjs.com/), plugins is not a huge dependency for 11ty. This will reduce your build time. However, the downside of this is that you probably couldn't find all the plugins you may need for your sites. Regardless, check out the list of 11ty plugins on npm [here](https://www.npmjs.com/search?q=eleventy-plugin).

### Easy, flexible templating

There are <a href="https://www.11ty.dev/docs/languages/">10 templating languages</a> to choose from. You can start off with the ones you're already familiar with, without having to learn a new templating language. Also, it is possible to mix any templating language as you see fit.

I typically use <a href="https://www.markdownguide.org/">Markdown</a> and <a href="https://mozilla.github.io/nunjucks/">Nunjucks</a> because they are really straightforward and easy to use. You can use a Nunjucks syntax in your Markdown file when you need it. For example, in your `.md` file, you can iterate over a collection or data with the Nunjucks `for loop` syntax, and then switches back to Markdown to render the content of the `for loop`.

### Flexible, dynamic data model
As any SSG, you would add data in a template front matter like this:

```js
// posts/post-1.md

---
title: "Hello world"
layout: "post-layout.njk"
---
Content of hello world
```

You can avoid from repeating the `layout` front matter config in all your posts by adding a `posts.json` in the directory of your posts. 
The `posts.json` file config tells 11ty to use `post-layout.njk` layout for all the `.md` file defined in the `/posts` directory. The `tags` turns the posts in this directory into a collection. Your posts can be read in `collections.posts`.

```js
// posts/posts.json
{
	"layout": "post-layout.njk",
	"tags": "posts"
}

// posts/post-1.md
---
title: "Hello world"
---

// posts/post-2.md
---
title: "Goodbye world"
---
```

There are more options on how to process your data, read more in their <a href="https://www.11ty.dev/docs/data-frontmatter/">documentation</a>.

### External data
You might want to integrate and access data from an external source, like a CMS for example. 11ty supports this with their custom data feature. All you need to do is to add your external data in the `_data` directory as a JS module or a JSON object. The file name is what are exported as the global variable that you can access in your templates files.

```js

// JS module example: _data/colors.js
module.exports = ["Red", "Green", "Yellow"];

// JSON object example: _data/colors.json
[
	"Red",
	"Green",
	"Yellow"
]
```
These data are then globally accesible in your template files as `colors`.

### Summary

There are more advanced features that I will not cover in this post ~~(before it get too boring and lengthy)~~. Anyway, in my opinion, 11ty has so much to offer due to it's highly **configurable**, **extensible** and **flexible** nature. I think you would appreciate the `autonomy` you will have when building a site with 11ty.
