---
title: "Setup Hugo Blog and gittalk"
date: 2023-01-27T08:20:00+08:00
draft: false
tags: ['self host']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Setup blog using hugo and Github Pages"
---

Setup Hugo blog and add comment function with gittalk.

Using github-style theme by Meik

<!--more-->
## Install Hugo

### Debian

`sudo apt install hugo`

## Configuration

Init site

```
$ hugo new site site-name
$ cd site-name
```

Use github-style theme

````
$ cd themes
$ git clone https://github.com/MeiK2333/github-style.git
````

Setup readme

```
hugo new readme.md
echo '`Hello World!`' > content/readme.md
#Set 'draft:true' to 'draft:false'
```

Add new post

```
hugo new post/title.md
```

Post template

Use `<!--more-->` to seperate content that will display in the posts page as abstraction and the rest of the content. This is different from summary, as summary will not appear in the post.

```
---
title: "template"
date: 2019-10-22T18:46:47+08:00
draft: false
tags: ['value_1', 'value_2']
---
abstraction show in the post page
<!--more-->
other content

or

---
title: "template"
date: 2019-10-22T18:46:47+08:00
draft: false
tags: ['value_1', 'value_2']
summary: "The summary content"
---
```

Modify config.toml

```
baseURL = "https://blogdomain.com"
languageCode = "zh-cn"
title = "XXX's blog"
theme = "github-style"
pygmentsCodeFences = true
pygmentsUseClasses = true

[taxonomies]
  category = 'categories'
  tag = 'tags'

[params]
  author = "XXX"
  description = "In solitude, where we are least alone."
  github = "username"
  facebook = "username"
  twitter = "username"
  linkedin = "username"
  instagram = "username"
  tumblr = "username"
  email = "email address"
  url = "https://domain.com"
  keywords = "blog"
  rss = true
  lastmod = true
  userStatusEmoji = "😀"
  favicon = "/images/github.png"
  location = "Somewhere"

  [[params.links]]
    title = "Link"
    href = "https://domain.com"
  [[params.links]]
    title = "Link2"
    href = "link"
    icon = "https://domain.com/images/avatar.png"

[frontmatter]
  lastmod = ["lastmod", ":fileModTime", ":default"]
```

Start server to preview

```
# start on port 1313 with github-style theme
$ hugo server -t github-style -D
# generate site in public directory
$ sudo hugo --gc --minify --cleanDestinationDir

```

## Config gittalk

Init a new repo for storing comments(enable issues)

Add new OAuth app in Settings>Developer settings>OAuth Apps

Add

```
[params]  
enableGitalk = true
[params.gitalk]
    clientID = "Your client ID" 
    clientSecret = "Your client secret" 
    repo = "repo name"
    owner = "repo's owner github username"
    admin = "github username"
    id = "location.pathname"
    labels = "gitalk"
    perPage = 15
    pagerDirection = "last"
    createIssueManually = false
    distractionFreeMode = false

```

to config.toml
