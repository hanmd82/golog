### A simple place for learning Go

Create a new Hugo site and download a simple theme
```
hugo new site golog

git submodule add https://github.com/jbub/ghostwriter themes/ghostwriter

git submodule init
git submodule update
```

Set up the content folder of the site with some basic content
```
cd content && mkdir -p project page post resource && cd ..

cp -R themes/ghostwriter/exampleSite/content/page ./content
cp themes/ghostwriter/exampleSite/content/project/my-awesome-project.md ./content/project/my-first-project.md
cp themes/ghostwriter/exampleSite/content/post/goisforlovers.md content/resource/go-template-primer.md
# edit as needed
```

Configure the Hugo site
```
cat << EOF > config.toml
baseurl = "/"
title = "Go|oG"
theme = "ghostwriter"
languageCode = "en-us"

[Taxonomies]
    tag = "tags"

[Params]
    mainSections = ["post"]
    intro = true
    headline = "Go|oG"
    description = "A place for learning Go"
    github = "https://github.com/hanmd82"
    twitter = "https://twitter.com/hanmd82"
    dateFormat = "Mon, Jan 2, 2006"

[Permalinks]
    post = "/:year/:month/:day/:filename/"

[[menu.main]]
    name = "Projects"
    url = "/project/"
    weight = 1

[[menu.main]]
    name = "Contact"
    url = "/page/contact/"
    weight = 2

[[menu.main]]
    name = "About"
    url = "/page/about/"
    weight = 3

[[menu.main]]
    name = "Blog"
    url = "/"
    weight = 4

[[menu.main]]
    name = "Resources"
    url = "/resource/"
    weight = 5
EOF
```
