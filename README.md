# amomorning.github.io

Personal academic page built using [al-folio](https://github.com/alshedivat/al-folio.git)

## Usage
### Environment
- [Ruby + Devkit](https://rubyinstaller.org/downloads/)
- Jekyll
- Bundle

``` bash
$ ridk install
# select 3
$ gem -v
# > 3.3.3

$ gem install jekyll bundler

$ jekyll -v
# > jekyll 4.2.1
```
### Install
``` bash
# shallow clone
$ git clone --depth 1 git@github.com:amomorning/amomorning.github.io.git
$ cd amomorning.github.io
$ bundle install
```

### Preview

Create a prebiew at http://127.0.0.1:4000.

``` bash
$ bundle exec jekyll serve
```

## Categories

### News
The recent update at **about** is in the  `_news` folder, includes **inline posts** and **long posts**
#### Inline posts
``` markdown
---
layout: post
date: 2021-03-30 11:00:00-0400
inline: true
---
```
#### Long posts

``` markdown
---
layout: post
title: Present "ArchIndex A Web-based and Data-driven Retrieval System for City Blocks" at CUPUM 2021
date: 2021-06-13 16:11:00-0400
inline: false
---

<!-- the content of the posts -->

````

### Publications
publications page is generated automatically from your BibTex bibliography. Simply edit `_bibliography/papers.bib`. You can also add new `*.bib` files and customize the look of your publications however you like by editing `_pages/publications.md`

#### Author
In publications, the author entry for yourself is identified by string `scholar:last_name` and string array `scholar:first_name` in `_config.yml`:

``` yml
scholar:
  last_name: Einstein
  first_name: [Albert, A.]
```
#### Coauthor
Keep meta-information about your co-authors in `_data/coauthors.yml` and Jekyll will insert links to their webpages automatically. 

``` yml
"Adams":
  - firstname: ["Edwin", "E.", "E. P.", "Edwin Plimpton"]
    url: https://en.wikipedia.org/wiki/Edwin_Plimpton_Adams

"Podolsky":
  - firstname: ["Boris", "B.", "B. Y.", "Boris Yakovlevich"]
    url: https://en.wikipedia.org/wiki/Boris_Podolsky

"Rosen":
  - firstname: ["Nathan", "N."]
    url: https://en.wikipedia.org/wiki/Nathan_Rosen

"Bach":
  - firstname: ["Johann Sebastian", "J. S."]
    url: https://en.wikipedia.org/wiki/Johann_Sebastian_Bach

  - firstname: ["Carl Philipp Emanuel", "C. P. E."]
    url: https://en.wikipedia.org/wiki/Carl_Philipp_Emanuel_Bach
```
## License

The theme is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
