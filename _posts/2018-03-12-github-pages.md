---
layout: post
title:  "Setup My GitHub Pages"
date:   2018-03-12 12:00:00 +0100
categories: blog github setup
comments: true
---

This is a quick reference on how I setup GitHub Pages with custom sub-domain.

### New GitHub repository
Create new repository with name `igochkov.github.io`. 

![Create new repository menu]({{ "/assets/2018-03-12-github-pages/new_repository_menu.png" | absolute_url }})

It is important that the first part of the repository matches exactly the username of the github account. Only then the repository will be seen as GitHub Pages repository.
 
![Create new repository]({{ "/assets/2018-03-12-github-pages/create_new_repository.png" | absolute_url }})

### Create landing page
Create new file with name `index.html` or `index.md` with the content "Hello World". Commit the new file.

![Create new file]({{ "/assets/2018-03-12-github-pages/new_file.png" | absolute_url }})

### Test GitHub Pages
 Open URL `https://igochkov.github.io`. You should see the content "Hello World".

### Change theme
Open settings of the current repository. Scroll to GitHub Pages block and press `Change theme` button. Select theme of choice. I left the default `Minima` theme.

![Setting theme]({{ "/assets/2018-03-12-github-pages/theme.png" | absolute_url }})

### Setup custom sub domain
Domain providers' DNS settings page - add CNAME record, with subdomain `blog`, domain `gochkov.com`

 ![CNAME]({{ "/assets/2018-03-12-github-pages/CNAME.png" | absolute_url }})
        
### Setup GitHub repository for custom domain 
 Go in Settings / GitHub Pages / Custom domain and specify `blog.gochkov.com` in the field.

 ![Custom domain setup]({{ "/assets/2018-03-12-github-pages/sub_domain.png" | absolute_url }})

### Test the custom domain
Open `http://blog.gochkov.com`