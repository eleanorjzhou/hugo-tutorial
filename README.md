# Hugo Tutorial

## Create custom theme

### Setup 
1. Create a new site: `hugo new site newSite`
2. Go into the new site folder: `cd newSite`
4. To serve the site, run `hugo server`

## Base template
First, create the `baseof.html` file (directory: `layouts/_default/baseof.html`). this is where all our pages will be rendered by default:
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta name="description" content="Description of your website">
        <title>
            {{ .Site.Title }}
        </title>
        
        <!-- Additional CSS and JavaScript files -->
        <link rel="stylesheet" href="/css/global.css">
        
        <!-- Favicon -->
        <link rel="icon" href="/path/to/your/favicon.ico">
    </head>
<body>
    <!-- Header -->
    {{ partial "header.html" . }}
    
    <!-- Your website content here -->
    {{ block "main" . }}
    {{ end }}

    <!-- Footer -->
    {{ partial "footer.html" . }}

    <!-- Additional JavaScript code -->
    <script>
        // Your JavaScript code here
    </script>
</body>
</html>
```

## index.html
If we need a separate landing page or homepage, we can create an `index.html` file at the root of the `layouts` folder and add the following code to render the main area instead:
```
# index.html

{{ define "main" }}

<!-- Partial for landing page layout -->
{{ partial "landing.html" }}

{{ end }}
```


## Menu
To configure a navigation menu, we can do the following:
1. Initialize the menu in the `config.toml` file:
```
# config.toml

sectionPagesMenu = 'main'
```

2. In the `header.html` file, we loop the menu items with this:
```
# header.html

<!-- Menu items -->
{{ range .Site.Menus.main }}
<a class="navbar-item" href="{{ .URL }}">
  {{ .Name }}
</a>
{{ end }}
```

3. We then need to create individual folders and `_index.md` files for pages of the menu items, so that each page can be detected and fetched. For example:
```
content
├── about
│   └── _index.html
├── blog
│   └── _index.html
└── documentation
│   └── _index.html
└── _index.html (for the homepage)
```
```
# about - _index.md

---
title: "About"
description: "This is the about page."
draft: false
weight: 3

---

This is the About page. Here, you can provide an introduction or overview of the content available within this section.
```

The menu item should then show on our navbar.


4. To make the pages available, we need to fetch the metadata and contents into the `_default/section.html` file, for example:
```
# section.html

<main>
  <h1>{{ .Title}}</h1>
  <h2>{{ .Description }}</h2>
  <p>{{ .Content }}</p>
</main>
```


## Partials
These are the components (e.g. header, footer) of the site. 
The partial html files will always live in the `/partials` folder.
To fetch a partial into the `_default` html files or the `index.html` file, we can use `{{ partial "partial.html" . }}`.
For example:
```
# baseof.html

<body>
    <!-- Header -->
    {{ partial "header.html" . }}
    
    <!-- Your website content here -->
    {{ block "main" . }}
    {{ end }}

    <!-- Footer -->
    {{ partial "footer.html" . }}

    <!-- Additional JavaScript code -->
    <script>
        // Your JavaScript code here
    </script>
</body>

```

## Blog Setup
We created a `section.html` file for a general layout of all the menu pages.
But now we need a separate layout for the Blog page that looks different than other pages and to display a list of blog posts.
To do so, we can:
1. Create a new folder named `blog` which corresponds with the one in the `/content/` folder
2. Create a new `section.html` file for a separate layout for the blog page:
```
# /layouts/blog/section.html

{{ define "main" }}

<section class="hero is-medium">
    <div class="hero-body">
      <p class="title">
        {{ .Title }}
      </p>
      <p class="subtitle">
        {{ .Description }}
      </p>
    </div>
</section>

<section class="section is-medium">

    {{ partial "article-list" . }}

</section>

{{ end }}
```

## List of articles
To get the list of articles, we can create a sparate partial html file `article-list.html` and do the following:
1. Fetch the list of articles:
```
# article-list.html

<ul>

  {{ range .Pages }}
    <li>
      <a href="{{ .Permalink }}">{{ .PublishDate.Format "January 2, 2006" }} - {{ .Title }}</a>
      <p>{{.Summary}}</p>
    </li>
  {{ end }}

</ul>
```

2. Import the article list partial into the page section:
```
# page-section.html

{{ $title := "Blog" }}
    {{ if eq .Title $title }}
    <section>
      {{ partial "article-list.html" . }}
    </section>
    {{ else }}
    <h1>Not blog</h1>
    {{ end }}
```


## Single article
To create the layout for a single blog post, we can use the `single.html` file in the `_default` folder:
```
{{ define "main" }}
<!-- Single post layout -->

<div class="container">

    <div class="section is-medium">

        <article class="content">

            <h1>{{ .Title }}</h1>
            <p>{{ .PublishDate.Format "January 2, 2006" }}</p>
    
            <hr>
    
            {{ .Content }}
    
        </article>

    </div>
    
</div>

{{ end }}
```

