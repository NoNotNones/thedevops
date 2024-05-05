---
title : "Github Pages"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

#### Overviews

{{%attachments style="grey" /%}}
{{%attachments title="Related files" pattern=".*\.(pdf|mp4|png|Dockerfile)$"/%}}

#### [Learning GitHub Pages](https://www.linkedin.com/learning/learning-github-pages/hosting-your-content-for-free-on-github?resume=false&u=103729754)

Lab Content: https://raybo.org/slides_publish/

Use Repository: https://github.com/LinkedInLearning/publish-github-2894570


#### [Getting Your Website Online](https://www.linkedin.com/learning/getting-your-website-online-18759497/setting-up-a-static-site-host?resume=false&u=103729754)

##### Setting up a static site host

Github Pages: https://pages.github.com/ 

![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/1.png?featherlight=false&width=40pc)

1- Create repo & Upload files

![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/2.png?featherlight=false&width=40pc)

2- Go to Settings:

Pages:

- Check website: 

![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/4.png?featherlight=false&width=40pc)
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/3.png?featherlight=false&width=40pc)

3- Create Sub website:

- Create **new repo: test-project**
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/5.png?featherlight=false&width=40pc)

- Crate a new file: **index.html**

```js
This is my index page!
```

- Go to Settings - Pages
  - Build and devployment
    - Source: Deloy form a branch
    - Branch: **main** -> Save
    - Refesh curent page

![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/6.png?featherlight=false&width=40pc)

- Check github websites:
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/7.png?featherlight=false&width=40pc)

- Reference: https://www.netlify.com
  - Overview: 
    - There's a free tier that allows you to host multiple websites, as well as other features, such as rolling back to previous versions
    - It could be connect to ant Git host
    - Can manual deploy : https://app.netlify.com/drop
- Setting up domain and host with a website builder: https://www.squarespace.com/
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/8.png?featherlight=false&width=40pc)
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/9.png?featherlight=false&width=40pc)
![534](/thedevops/images/5-learn/5.3-github/5.3.4-pages/10.png?featherlight=false&width=40pc)

- Publishing projects on social coding platforms
![534][10]


[10]: /thedevops/images/5-learn/5.3-github/5.3.4-pages/10.png?featherlight=false&width=40pc
