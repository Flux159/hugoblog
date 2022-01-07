---
title: New Hugo Blog
date: 2022-01-06
categories:
  - Experiments
tags:
  - experiments
image: hugoblog.png
---

# Updating blog from gatsby to hugo

When I previously wrote about hosting a [static site on github pages](https://suyogs.com/p/experiments/github-pages-for-static-sites/), I used [Gatsby](https://www.gatsbyjs.com/) as the content generation system.

After a painful few days updating gatsby at the end of 2021, I decided to move off of Gatsby and over to Hugo.

The primary reason was because updating Gatsby through NPM caused a signficant number of breaking changes in the github action that published the site. This was unacceptable since the amount of time spent debugging the build process with upgrades could have been spent on more productive things.

I quickly looked for alternatives and found [Hugo](https://gohugo.io/), a Go-based static site generator that doesn't use NPM for updates (Hugo is delivered as a static binary and doesn't rely on the NPM / JS ecosystem). 

Migrating to Hugo took about 3 hours and in the process I got a new blog theme that supports dark mode, migrated the share buttons (only viewable on desktop at the moment), and deployed the site using github actions again. All in all a relatively painless experience.

A large benefit was that hugo generation is super fast - milliseconds in development mode on my laptop & the total github action build time (from checking out to building to deploying) is around 30s to 1 minute.

A con is that writing [React/JS based code in hugo](https://gohugo.io/hugo-pipes/js/) seems a little bit more difficult than in Gatsby (it uses esbuild which I'm a bit unfamiliar with compared with webpack), but at least it's still available as an option.
