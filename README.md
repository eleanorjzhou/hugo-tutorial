# Hugo Tutorial

## Create custom theme

### Setup 
1. Create a new site: `hugo new site newSite`
2. Go into the new site folder: `cd example`
3. Create a theme: `hugo new theme customTheme`
4. To serve the site, run `hugo server`

## Base template
First, create the `baseof.html` file (directory: `layouts/_default/baseof.html`). this is where all our pages will be rendered by default:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="text/html, width=device-width, initial-scale=1">
    <title>{{ block "title" . }}
      <!-- Blocks may include default content. -->
      {{ .Site.Title }}
    {{ end }}</title>
  </head>
  <body>
    <!-- Code that all your templates share, like a header -->
    {{ block "main" . }}
      <!-- The part of the page that begins to differ between templates -->
    {{ end }}
    {{ block "footer" . }}
    <!-- More shared code, perhaps a footer but that can be overridden if need be in  -->
    {{ end }}
  </body>
</html>
```

## index.html
If we need a separate landing page or homepage, we can use the `index.html` file instead.

## Archetypes


### Menu
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
The menu item should then show on our navbar.

3. We then need to create individual folders and `_index.md` files for pages of the menu items, for example:
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

This is the landing page. Here, you can provide an introduction or overview of the content available within this section.
```
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
# section.html

<body>
    <!-- Header -->
    {{ partial "header.html" . }}
    
    <!-- Your page content here -->
    {{ partial "page-section.html" . }}

    <!-- Footer -->
    {{ partial "footer.html" . }}

    <!-- Additional JavaScript code -->
    <script>
        // Your JavaScript code here
    </script>
</body>

```




