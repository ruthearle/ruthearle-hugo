+++
author = "Ruth Earle"
categories = ["tutorial", "hugo"]
date = "2016-04-30T14:04:32+01:00"
description = "How I got Hugo Static Site Generator up and running."
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "I Go With Hugo"
type = "post"

+++

So I finally have my blog up and running. Part of the delay was that I could not find a platform with templates that I liked the other part was sheer laziness. Then I found [Hugo][1].

Hugo is written in Go and is blazingly fast at building static content. There are [beautiful templates][2] to get you started and it was relatively easy to get everything onto Github.

The [website][1] states 

>'Hugo is quite possibly the easiest to install..., simply download and run'. 

This was true, but Hugo does not come prepackaged with a theme so when you run
`hugo server` and go to `http://localhost:1313` (default port) you are presented with a blank page or a page of XML if you have added content. Hmmm...doc's!

The [docs][1] are thorough and covers most things but I could not [load css](#css). I followed the tutorial but after much frustration I googled for another answer.

So with the help of [Hugo tutorial][3] and [RH.io][4] I came up with a workflow that got my blog onto [Github pages][5] with a custom domain name ([click here](#domain) to see how to add a custom domain). Let's go.

### Install Hugo - Mac
Ensure you have Homebrew installed and run 
```bash
$ brew update && brew install hugo
```
To install Hugo on other OS's follow instructions [here][6].

### Git repo's
Hugo compiles your static site into a `public` directory within the root. The good news is that we can configure the path to whatever directory you want.
Github pages will host static files from a repository that meets two conditions:

1. The repository is named `<username>.github.io`
2. The repository includes a `index.html` file.

The first thing to do is create two git repositories:

  * `<username>-hugo.git` - the root of your Hugo site
  * `<username>.github.io.git` - this will be used to house the compiled static pages for your site, which ordinarily would go in `/public`.

*Replace `<username>` with your github username.*

### Create your Hugo site
On the command line run:
```bash
~ $ hugo new site <username>-hugo
~ $ cd <username>-hugo
```
Add a page `hugo new about.md`. This page will be created in the `/content` directory. Complete this page with all the necessary information.

Next add the `<username>-hugo.git` repository remote address
```bash
~/<username>-hugo $ git init
~/<username>-hugo $ git remote add origin git@github.com:<username>/<username>-hugo.git
```

Next add a submodule for the `<username>.git.io repository` within your `<username>-hugo` directory.
```bash
~/<username>-hugo $ git submodule add origin git@github.com:<username>/<username>-github.io.git
```

At this point it might be a good idea to choose [a theme][2].  I found I had to install the theme as a submodule otherwise git complained:
```bash
~/<username>-hugo $ mkdir themes && cd themes
~/<username>-hugo/themes $ git submodule add https://github.com/appernetic/hugo-bootstrap-premium
```
Follow the instructions for setting up your theme. This usually involves copying the themes config.toml/yaml/json to the root of your site directory.

### Configuration<a id="css"></a>
In the config.toml ensure you set the following parameters:

1. `baseurl = "http://yourwebsite.com"`
  * set this to your github pages url e.g. `http://<username>.github.io`. If you are going to use a [custom domain name with github pages][7] use that. The `baseurl` parameter is used to set the hostname and path to the root. When you run your site locally with `hugo server` the `baseurl` parameter is overridden and set to `http://localhost:1313/` so setting it to localhost will cause your website to be unnavigable when compiled.
2. `canonifyurls = true`
 * this is used to set a relative path for your urls. Default is false.
3. `publishdir = <username>.github.io`
 * this parameter tells Hugo where to compile the static site. The default location is `/public`. Setting this to your github site repo will make deploying your site a bit easier.
4. `[params] customCSS/JS`
 * set this parameter to `[default]`, which will load the themes CSS. But do check the instructions for your chosen theme as it may employ a different strategy.

 The remaining parameters are going to be personal to you so [read the docs][8] and get familiar with what the parameters do.

### Check the site locally
Spin up the site with `hugo server` and in your browser go to `http://localhost:1313`. If everything looks super then it's time to get the site live.

### Build and deploy <a id="domain"></a>
Next build the static pages:
```bash
~/<username>-hugo/themes $ cd ../
~/<username>-hugo/ $ hugo
```
Commit and push these changes to git:
```bash
~/<username>-hugo/ $ git add -A
~/<username>-hugo/ $ git commit -m "first commit"
~/<username>-hugo/ $ git push origin master
```
##### Custom domain
Adding a custom domain is as simple as creating a file with one line. Ensure you have added a CNAME record on your DNS and create a file in the root of your github pages directory:
```bash
~/<username>-hugo/ $ cd <username>.git.io
~/<username>.git.io/ $ touch CNAME
```
Open the newly created  `CNAME` file and add your custom damain name at the top of the file, i.e. `blog.ruthearle.com`.

You are now ready to push the static pages to git:
```bash
~/<username>.git.io/ $ cd <username>.git.io
~/<username>.git.io/ $ git add -A
~/<username>.git.io/ $ git commit -m "first commit"
~/<username>.git.io/ $ git push origin master
```

Once all changes are pushed your site should be available to view at `<username>.git.io` or your custom domain.

Enjoy!

[1]: https://gohugo.io/
[2]: http://themes.gohugo.io/
[3]: https://gohugo.io/overview/quickstart#step-8-customize-robust-theme
[4]: http://rajeshhegde.com/2016/09/set-up-hugo-with-github-pages/
[5]: https://pages.github.com/
[6]: https://gohugo.io/overview/installing/
[7]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/
[8]: https://gohugo.io/overview/configuration/
