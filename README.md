
## Hugo blog

Most of this can be found on hugo quickstart page: https://gohugo.io/getting-started/quick-start/

There's also this for the theme: https://docs.stack.jimmycai.com/getting-started.html

Install hugo on mac:
```
brew install hugo
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

TODO: Update regular blog to publish properly to suyogs.com (couple of changes needed & new github actions)

