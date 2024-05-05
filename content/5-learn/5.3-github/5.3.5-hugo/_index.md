---
title : "Hugo"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.3.5 </b> "
---


#### [Learning Static Site Building with Hugo][hugo]


The Hugo "Learn" theme is a fantastic choice if you're looking to create a static website, particularly one focused on documentation, tutorials, or knowledge bases

#### Install themes
Source : https://github.com/matcornic/hugo-theme-learn

- cd /themes
- git clone https://github.com/matcornic/hugo-theme-learn.git

#### Basic configuration

- config.toml (hugo.toml)

```sh
# Change the default theme to be use when building the site with Hugo
theme = "hugo-theme-learn"

# For search functionality
[outputs]
home = [ "HTML", "RSS", "JSON"]
```

#### Create your first chapter page

hugo new --kind chapter basics/_index.md

```sh
+++
title = "Basics"
date = 2024-03-26T08:30:26+07:00
weight = 5
chapter = true
pre = "<b>X. </b>"
+++

### Chapter X

# Some Chapter title

Lorem Ipsum
```

#### Launching the website locally

- hugo serve
- Go to http://localhost:1313

[hugo]: https://www.linkedin.com/learning/learning-static-site-building-with-hugo-2/how-static-sites-work?resume=false&u=103729754


[1]: /thedevops/images/5-learn/5.3-github/5.3.4-pages/1.png?featherlight=false&width=40pc
