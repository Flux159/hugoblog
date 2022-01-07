
## Hugo blog

Most of this can be found on hugo quickstart page: https://gohugo.io/getting-started/quick-start/

There's also this for the theme: https://docs.stack.jimmycai.com/getting-started.html

Install hugo on mac:
```
brew install hugo
```

### Cloning

This repo uses submodules, so you should clone like this:
```
git clone --recurse-submodules https://github.com/Flux159/hugoblog.git
```

### Development

```
hugo server -D
```

Then go to http://localhost:1313/

### Building

Locally:
```
hugo -D
```

Then:
```
cd public
python3 -m http.server 1313
```

### Automated builds on github to github pages

There's a build & deploy script in the `.github/workflows/deploy.yml` file that automatically deploys to gh-pages & makes the site available at https://suyogs.com
