README.md

## Updating about page
You can plug in different contact for the about page at `content/_index.md`. For styling changes, contact the webmaster ;-D

## Add a new blog post
Go to the project root and type:
```
hugo new blog/[title of blog post].md
```

This will create a new blank post with the following 'frontmatter', or metadata about the post:

```
--- <-- Indicates opening of frontmatter

title: "First Blog Post" # Title of post; default is the filename
date: 2022-10-29T16:36:16-05:00
slug: "" # add something here if you want the blog post name in the URL to be something besides its filename
description: ""
keywords: []
draft: true              # Change to `false` once the post is ready to be published
tags: []                 # Categorize your post with tags
math: false              # Uses math typesetting
toc: false               # Includes a table of contents on screens >1024px

--- <-- Closing of frontmatter; write blog post under this line
```

## Preview blog
To preview your blog, start the hugo development server with the following command:
```
hugo server -D
```

Preview your changes live at `localhost:1313`

## Edit blog posts
Access your blog posts under the `content/blog` directory and edit the markdown files directly.