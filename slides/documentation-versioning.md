---
marp: true
title: Documentation Versioning with Hugo & Netlify
theme: gaia
size: 16:9
paginate: true
author: Nate Waddington
date: 2021-01-30
---
<!-- _paginate: false -->
<br />
<br />

# <!-- fit --> Documentation Versioning with Hugo & Netlify

<br />
<br />
<br />

__Nate Waddington__  
nwaddington@cncf.io

<br />

![width:300](../assets/cncf-color.png)

---

# Introduction

---

<!-- Maintainer: how hard is it to update when it's time to cut a new version? -->
<!-- Compartmentalization: Does all of the site need to be versioned? -->
<!-- How do we avoid versioning the entire site if only Documentation versions are the goal? -->
<!-- Searchability: Does the duplication of pages affect search results? -->
<!-- Switchability: How easy is it to change versioning schemes -->

# Versioning Concerns

What are the concerns when versioning documentation in a website?
- Do you need to version your documentation yet? 
- Navigation (Visitor ease of understanding)
- Maintainer ease of updates
- Localization / Internationalization
- Searchability
- Compartmentalization
- Switchability

---

<!-- Because Hugo is a static site generator, and -->
<!-- Netlify abstracts a lot of the server away -->
<!-- Query Strings and Cookies don't really work for a Hugo / Netlify site -->

# Versioning Schemes

For a Hugo / Netlify setup:

| friendly schemes     | unfriendly schemes |
| :---                 | :---               |
| None (don't version) |                    |
| Folder based         | Query Strings      |
| Subdomain based      | Cookies            |

---

<!-- Scores high on: - Maintainer ease of updates - Compartmentalization -->
<!-- This is probably the easiest way to start versioning. -->
<!-- Scores low on: - Localization / Internationalization -->

# Folder based

Each version of the documentation is placed in its own folder

```
content
└── docs
    ├── _index.md
    ├── v1
    ├── v2.2
    │   ⋮
    └── v3.4.0
```

![Folder based](../assets/folder-based-etcd.png)

---

<!--
One of the things that make this a good example is how Batard (2020, L8-L18)
manages the navigation in the `/site/layouts/docs/versions.html` file,
particularly the use of the `replace` function to ensure that when the links in
the dropdown menu are built, the versioned links reflect the currently loaded
page:
-->

# Folder based navigation code example

[velero.io](https://velero.io/) `/site/layouts/docs/versions.html`
```html
<div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
    {{ $original_version := printf "/%s/" .CurrentSection.Params.version }}
    {{ $latest_url := replace .Params.url .CurrentSection.Params.version .Site.Params.latest | relURL }}
    {{ $currentUrl := .Permalink }}

    {{ range .Site.Params.versions }}
        {{ $new_version := printf "/%s/" . }}
        <a class="dropdown-item"
           href="{{ replace $currentUrl $original_version $new_version | relURL }}">{{ . }}</a>
    {{ end }}
</div>
```
Batard (2020, L8-L18)

---

<!--  Scores high on: - Localization / Internationalization -->
<!-- Scores low on: - Compartmentalization -->

# Subdomain based

![Subdomain based](../assets/subdomain-based-k8s.png)

---

Subdomain based sample

---

# Questions?

---

# References / Credits

[Batard, T.](https://github.com/tbatard) (2020, August 13). _vmware-tanzu/velero_. GitHub. Retrieved January 19, 2021 from https://github.com/vmware-tanzu/velero/blob/db403c6c54b0048fada2b5db628c44be4ac0fd79/site/layouts/docs/versions.html

