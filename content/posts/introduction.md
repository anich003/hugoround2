---
title: "Introduction to Hugo and Static Site Generation"
date: 2021-03-13T15:20:19-08:00
draft: false
---

### Steps to create project
1. Start a new hugo "site" by running `hugo new site <name>`
2. `cd <name>`
3. Add a theme by cloning a theme repo into `/themes` then adding line to config.toml
4. Add content with `hugo new <content>/<name-of-article>.md`
5. Build static pages with `hugo -D` which will build to `/public/` by default
