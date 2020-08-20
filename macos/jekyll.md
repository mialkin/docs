# Jekyll

Create a new Jekyll site.

```sh
jekyll new <website>
sudo chown -R <username> <website>
cd <website>
```

Run website with page refresh on file change and logs output to console.

```sh
jekyll serve --livereload --trace --port 4000
```

## gitignore file

```text
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata
```
