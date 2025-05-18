
## Hugo blog

Most of this can be found on hugo quickstart page: https://gohugo.io/getting-started/quick-start/

Install hugo on mac:
```
brew install hugo
```

Currently using:
```shell
$ hugo version
hugo v0.147.3+extended+withdeploy darwin/arm64 BuildDate=2025-05-12T12:25:03Z VendorInfo=brew
```

### Cloning

```
git clone https://github.com/Flux159/hugoblog
```

### Development

```
bun run dev
```

Or do `hugo server -D` if want to run manually w/out bun.

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
