---
title: "Fixing Syntax Highlighting Issue in Hugo with Netlify"
date: 2020-05-11T14:35:25+05:30
draft: false
categories: ["articles"]
keywords:
  - hugo
  - personal-site
  - fixing
  - netlify
---

For a longtime I was facing issues with syntax highlighting in my personal site. In my local environment when I build hugo files, it does work perfectly. But whenever it is deployed to Netlify, it does not.

After a bit of searching it does come to a solution. Just need to add a `netlify.toml` file in the project root.

<!--more-->

> I was helpful by reading [Hugo Netlify - No Code Syntax Highlighting](http://brianyang.com/hugo-netlify-no-code-syntax-highlighting/) by [@Brian Yang](https://twitter.com/brianyang)

```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.70.0"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --gc --minify --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.70.0"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.70.0"

[context.branch-deploy]
command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.70.0"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```

This was explained clearly in the Hugo docs but I thought it wasn't relevant.
https://gohugo.io/hosting-and-deployment/hosting-on-netlify/
