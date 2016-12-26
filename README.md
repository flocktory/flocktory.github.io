## Writing docs
English docs in [_docs_ru](docs_ru)
Russian docs in [_docs_en](docs_en)

Each doc should start with [front matter](http://jekyllrb.com/docs/frontmatter/). Example
```
---
layout: doc
title:  "Emails"
date:   2016-12-06 16:34:18 +0300
section: emails
---
```

* `layout` always `doc`
* `title` the name of document
* `date` date of creation
* `section` one of sections defined in [_config.yml](_config.yml). If you need more sections add it to config file.

### Liquid tags in document
TBD

## Development
This repo uses Jekyllrb.
[install instructiuons](http://jekyllrb.com/docs/installation/)

To run server:
```
jekyll serve
```
Open [http://localhost:4000](http://localhost:4000)
