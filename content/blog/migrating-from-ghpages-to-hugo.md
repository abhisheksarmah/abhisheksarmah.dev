---
title: Migrating my site from ghpages to Hugo
slug: migrating-my-site-from-ghpages-to-hugo 
date: 2020-04-20
draft: false
categories: ["articles"]
keywords:
  - hugo
  - personal-site
  - ghpages
---

Recently I have moved my personal site from ghpages to Hugo. Previously it was a single [index.html](https://github.com/abhisheksarmah/abhisheksarmah.github.io) page, which was deployed through ghpages. At that time I dont even have a domain name purchased for my personal site. 

By the end of March, I had a thought rebuilding my own personal site with my own blog. So, I add it to my todo list. It's only been a month since I started writing articles in [dev.to](https://dev.to/abhisheksarmah), but having your own website gives you more customisation and much more learning in the journey. And I also wanted to place my all of works and projects I am doing in the same.

So, let's come to the point...

<!--more--> 


## As tweeted, I will be sharing 
- Why static site generators?
- Why hugo?
- Learning hugo for my site
- Deploying to production
- Roadmaps

<!-- {{< tweet 1251569080819015681 >}} -->


## But wait! Let's finish my journey of choosing Hugo :rofl:

Once I got to read the article of Sebastian De Deyne's about [migrating his personal site from ghpages to Hugo](https://sebastiandedeyne.com/migrating-my-site-to-hugo/). From that blog I got reference of Sara Soueidan's article about [Migrating from Jekyll+Github Pages to Hugo+Netlify](https://www.sarasoueidan.com/blog/jekyll-ghpages-to-hugo-netlify/). Going through the above articles, and knowing about SSG's and Hugo for the first time it set me a poitive mindset about learning Hugo.

You might be thinking of being a laravel developer, why I go with Hugo (go based templating) instead of any SSG's from PHP like Jigsaw (blade based templating). I have heard about Jigsaw earlier, but not gathered any knowledge about it. When I was learning Hugo and going through the internet a while, I came to know about the usage of Jigsaw (by that time I had done building first phase of my site and published it to the internet). Actually I am not comparing both Jigsaw and Hugo, but for a developer coming from the laravel world and having familiarity with blade templating gives him more power to go with Jigsaw. If you are familiar with laravel, you should definitely check this guide about [How to use Jigsaw to quickly and easily build static sites](https://www.freecodecamp.org/news/how-to-use-jigsaw-to-quickly-and-easily-build-static-sites-8a3304c3ad7e/).

And for me, I always welcome learning new things. The [learning resources](https://gohugo.io/getting-started/external-learning-resources/) provided alongside the docs gives me a great starting point. I also checked out [Sebastian's github repo](https://github.com/sebastiandedeyne/sebastiandedeyne.com) and collected some of the parts from the repo regarding how he uses the blog's single page and blog list's pagination page. And there it starts my journey...

## Why static site generators?

Static Site Generators are a new, hybrid approach to web development that allow you to build a powerful, server-based website locally on your computer but pre-builds the site into static files for deployment. It doesn’t change between the point when the developer hits the “save” button, and when the end user clicks on it. Developers create the content using HTML, format it with CSS, and upload the static page to a server where it remains unchanged and ready to be accessed by a browser request.

Benifits of using SSG's-

- **Speed -** Static sites are fast! Since there are no database queries, no templates to render, and no client-server requests to process, because all the web server needs to do is return a file.

- **Secure -** With SSG already prebuild on the server, it reduces the risk of any attacker since there is no query, no request processing in the server, there is nothing dynamic that can be exploited.

- **Cheap -** The cost of hosting of SSG are are cheap. As there is no processing requests like querying a database, loading a dynamic content on the web server, so it reduces the cpu load, memory intake in the webserver. So, we could easily host it on free hosting providers like ghpages, netlify etc. 

- **Scalable -** A single server can handle a lot of traffic if it’s just serving static files. However, if we need more resources we can load balance traffic across multiple servers. This is relatively easy to set up as we just need to make sure the static site is on all our servers.

You could read more about it [here](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/).

## Why Hugo?

Hugo is a static site generator written in Go. It is optimized for speed, easy use and configurability. Hugo takes a directory with content and templates and renders them into a full html website.

Hugo makes use of Markdown files with front matter for meta data.

A typical website of moderate size can be rendered in a fraction of a second. A good rule of thumb is that Hugo takes around 1 millisecond for each piece of content.

It is written to work well with any kind of website including blogs or docs.

## Learning Hugo for my site

As I was new to Hugo, gained my Hugo knowledge from docs and the articles that I have mentioned above. I wanted to make my website as simple as possible. My site consist of `home`, `about`, `projects`, `work` and `hire me`. And definitely there will be `articles` section.

I will be talking about those folder structures, which I need in my journey. For detailed information regarding the Hugo folder structure you could follow the external resources that have been provided with the Hugo docs.

You could read more about Hugo directory structure [here](https://www.sarasoueidan.com/blog/jekyll-ghpages-to-hugo-netlify/)

After the fresh installation of a Hugo project, it gives me. 

![hugo install image](/blog_images/hugo_install.png "hugo directory")

I got used to `content`, `layouts`, `static` and the `config.toml` file. 

When starting a new project it always come to the point of configuration. `config.toml` contains the site’s configuration variables such as `baseURL` among many other variables that you may or may not decide to use. 

My `config.toml` looks like:

```toml
baseURL = "https://abhisheksarmah.dev"
languageCode = "en-us"
title = "Abhishek Sarmah - Fullstack web developer, open-source maintainer, blogger"

enableEmoji = true

[markup]
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    hl_Lines = ""
    lineNoStart = 1
    lineNos = false
    lineNumbersInTable = true
    noClasses = true
    style = "solarized-dark"
    tabWidth = 4
```

Now let's talk about `layouts` directory, where our website's master layout is stored. 

My layout directory looks like:

![layout directory image](/blog_images/layouts_dir.png "layouts directory")

Firstly, the homepage **`layouts/index.html`**, blog's paginated page **`layouts/_default/list.html`** and blog's single page **`layouts/_default/single.html`** will make use of baseof layout **`layouts/_default/baseof.html`** as its master layout to render contents.

My `baseof.html` page looks like:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
    <link rel="manifest" href="/site.webmanifest">
    <meta name="msapplication-config" content="/browserconfig.xml" />
    <meta name="msapplication-TileColor" content="#ffffff">
    <meta name="theme-color" content="#ffffff">
    <link rel="dns-prefetch" href="//fonts.googleapis.com" />
    <link rel="dns-prefetch" href="//unpkg.com" />
    <link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Aleo&display=swap" rel="stylesheet"/> 
    {{ if not (eq .Title $.Site.Title) }}
      <title>{{ .Title }} — {{ $.Site.Title }}</title>
    {{ else }}
      <title>{{ $.Site.Title }}</title>
    {{ end }}
    {{ block "meta" . }}
      <meta name="description" content="I'm a full stack web developer. I build websites & interfaces with JavaScript, CSS and PHP." />
      <meta property="og:title" content="{{ .Title }}" />
      <meta property="og:type" content="website" />
      <meta property="og:locale" content="en_US" />
      <meta property="og:url" content="{{ .Permalink }}" />
      <meta property="og:image" content="{{ "img/me_cover.png" | absURL }}" />
      <meta property="og:description" content="I'm a full stack web developer. I build websites & interfaces with JavaScript, CSS and PHP." />
    {{ end }}

    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=UA-151880965-4"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());

      gtag('config', 'UA-151880965-4');
    </script>
    
    <style>
      body {
        margin: 0;
        font-family: "Aleo", serif;
      }
    </style>
    {{ block "style" . }}

    {{end}}
  </head>
  <body class="antialiased min-h-screen flex flex-col">
    {{ partial "nav.html" . }}
  
    {{ block "main" . }}
  
    {{end}} 

    {{ partial "footer.html" . }}
  </body>
</html>
```


>As you are at this stage, I assume you know about the basics of hugo variables, templates, frontmatter, pages, functions etc. I am not going to review about these in this blog, becuase it will get more landy. You could check out the resources I have provided above, for learing more about Hugo.

As you could see above, in the body tag I have used two partials named `nav.html` and `footer.html` which is located in `partials` folder of layouts directory. The `main` block will render our contents that we will define in our `index.html` or blog contents.

Let's check out to the `content` directory. Rather than the the layouts and homepage, the content directory consist of all contents that is available to my site ranging froms `about`, `work`, `projects`, `hire` to the `blogs`.

The content directory looks like:

![content directory image](/blog_images/content_dir.png "content directory")

As you can see `about.html`, `work.html`, `projects.html`, `hire.html` is situated in the root of content directory. All of these files will be static files that get served statically (as-is, no modification) on the site root. By seeing the frontmatter of the below file, you could make a good guess about what I am talking. For static files its type is defined as `static` and its master layout is situated in `layouts/static/single.html`. Let's take an example of the `about.html` file which looks like:

```html
+++
type= "static"
page= "static/single.html"
title= "About Me"
description = "About Abhishek Sarmah - Fullstack web developer, blogger and food lover"
+++

<main class="max-w-2xl mx-auto my-10 text-xl px-6"> 
       <img alt="Abhishek Sarmah About Me Photo" class="mx-auto w-24 h-24 rounded-full mb-8" decoding="auto" sizes="100px" src="/img/me_about.jpg" style="object-fit: cover;"> 
       <div class="text-center">
          <h2 class="text-4xl font-bold tracking-wide mb-6">
              About Me
          </h2> 
        <p class="mb-4">
          I'm a sofware engineer from Guwahati, India. Currently associated with <a href="https://sumato.global" class="border-b border-gray-900 pb-px md:pb-1 text-gray-900">Sumato Globaltech Pvt. Ltd.</a> as a Senior Software Engineer.
        </p>
            <p class="mb-4">
              My passions are <a href="https://github.com/abhisheksarmah" class="border-b border-gray-900 pb-px md:pb-1 text-gray-900">open-source</a>, learning new things, managing software teams, and creating quality and maintainable products.
            </p>
            <p class="mb-4">
              I'm currently working on <a href="https://morselo.dev" class="border-b border-gray-900 pb-px md:pb-1 text-gray-900">Morselo</a>, where developers could store and manage thier valuable code snippets with ease.
            </p> 
            <p class="mb-4">
              I share everything I know about making softwares through my articles and tweets. Follow me on twitter at <a href="https://twitter.com/theasarmah" class="border-b border-gray-900 pb-px md:pb-1 text-gray-900">@theasarmah</a> for any tech related updates. 
            </p> 
        </div>
</main>
``` 

Now, let's come to the `blog` directory of content directory, here I list all of the blogs that I want to share about (definitely in md). It automatically rendered as a paginated list in blogs page, as we have already defined it in the `layouts/_default/list.html` page.

Let's check one of my blog:
```md
---
date: 2020-04-18
title: Why I started writing blogs 
categories: ["articles"]
keywords:
    - writing
---

I always hated of writing things down. But working as software developer, I read many blog posts of others about any technical ground. 

But it cames to a point where it might not always be helpful through reading others articles. You might get a different way of getting the things done. You could write your own implementation of the work which is already available. There are no hard and fast rule that you can not write about which is already available in the internet.

<!--more-->

Writing gives you more power of learning the topics better. So, I started to write about the technical difficulities, setup or any best articles I found out during my development phase. I also believe that somone will get benefitted by my writing. By writing I could also sharpen up my writing skills. 

My english is not so good, I am trying hard to improve it but I might be wrong somewhere because of grammar. Any suggestion which will help me, writing better.
```

As you could see above, I am using some custom frontmatters like `categories` and `keywords` for having more customisation in my blog.

>Now, you could checkout [my github repo](https://github.com/abhisheksarmah/abhisheksarmah.dev) for detailed view of the project. If you are not sure with any of the setup steps, checkout the Hugo docs.

As we have completed building our site, now lets come to building and deploying our site.

## Deploying to Netlify

I am using netlify, as it gives me zero config deployment to production. For all of the fontend projects I work, I always go with Netlify.

Before deploying we need to add a `_redirects` file in the root of contents folder for defining our redirect rule which contains:
```
/* /404.html 404
```

Now, its time to deploy it to Netlify. 

- Create an account on netlify.com if you don't have any
- Link your Netlify account to your code repository. Mine is hosted on Github so I just connected it (you do all this from the Netlify dashboard).
- Specify the build destination folder as well as a build command. `hugo` is the build command I used, and `public` is the build directory.
- Set up a custom domain. This also includes making some DNS changes.
- It took literally only 3 clicks to get an SSL certificate and HTTPS running for the site. 

Now enjoy your own personal site :thumbsup:

## Roadmaps of my personal site

- Use tailwindcss via npm instead of cdn.
- Use purgecss for removing unused css files
- add a blogroll section
- add comment section
- make ads working
- checkout the performance measures
- configure netlifycms to work with my site