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
<!-- Switchability: How easy is it to change versioning schemes -->
<!-- Searchability: Does the duplication of pages affect search results? -->

# Versioning Concerns

What are the concerns when versioning documentation in a website?
- Does the documentation need versioning yet? 
- Navigation (visitor ease of understanding)
- Maintainer ease of updates
- Localization / Internationalization
- Compartmentalization
- Switchability
- Searchability

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

![Folder based](../assets/folder-based-etcd.png)

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

velero.io `velero/site/config.yaml`
```yaml
  versioning: true
  latest: v1.5
  versions:
    - main
    - v1.5
    - v1.4
    - v1.3.2
    ⋮
```
Brubaker et al. (2020, L14-L20)

---

<!--  Scores high on: - Localization / Internationalization -->
<!-- Scores low on: - Compartmentalization -->

# Subdomain based

![Subdomain based](../assets/subdomain-based-k8s.png)

Uses git feature branches rather than folders to organize versions

Separate sites need to be setup in Netlify for each version supported

The subdomain scheme uses some simpler template code to generate links, only having to update the `.url`, but the Hugo config file is made more complex.

---

# <!-- fit --> Subdomain based navigation code example

https://kubernetes.io `website/layouts/partials/navbar-version-selector.html`

```html
<div class="dropdown-menu dropdown-menu-right" aria-labelledby="navbarDropdownMenuLink">
	{{ $p := . }}
	{{ range .Site.Params.versions }}
	<a class="dropdown-item" href="{{ .url }}{{ $p.RelPermalink }}">{{ .version }}</a>
	{{ end }}
</div>
```

Pursley et al. (2020, L4-L9)

---

https://kubernetes.io `website/config.toml` 
```toml
[[params.versions]]
fullversion = "v1.20.0"
version = "v1.20"
githubbranch = "v1.20.0"
docsbranch = "master"
url = "https://kubernetes.io"

[[params.versions]]
fullversion = "v1.19.4"
version = "v1.19"
githubbranch = "v1.19.4"
docsbranch = "release-1.19"
url = "https://v1-19.docs.kubernetes.io"
```
Bannister et al. (2020, L180-L192)

---

<!-- _paginate: false -->
<br />
<br />

# Questions?

---

# References / Credits

- [Bannister, T.](https://github.com/sftim) et al. (2020, December 23). kubernetes/website. GitHub. Retrieved February 2, 2021 from https://github.com/kubernetes/website/blob/7462297ee388332a7b0d27625929fbf44d0c1ea9/config.toml#
- [Batard, T.](https://github.com/tbatard) (2020, August 13). _vmware-tanzu/velero_. GitHub. Retrieved January 19, 2021 from https://github.com/vmware-tanzu/velero/blob/db403c6c54b0048fada2b5db628c44be4ac0fd79/site/layouts/docs/versions.html

---

- [Brubaker, N.](https://github.com/nrb), [Rosland, J.](https://github.com/jonasrosland), [Thompson, C.](https://github.com/carlisia), [Batard, T.](https://github.com/tbatard) (2020, September 16). _vmware-tanzu/velero_. GitHub. Retrieved February 2, 2021 from https://github.com/vmware-tanzu/velero/blob/1fd49f4fd66ecf6cd959ce258efbd9a549d8902b/site/config.yaml
- [Pursley, B.](https://github.com/brianpursley), [Horgan, C.](https://github.com/celestehorgan), [Takahashi, S.](https://github.com/shuuj3) (2020, July 21). _kubernetes/website_. GitHub. Retrieved February 2, 2021 from https://github.com/kubernetes/website/blob/072d4b41b45f5311538c24d375432a755f9e3f4c/layouts/partials/navbar-version-selector.html

