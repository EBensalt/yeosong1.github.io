# Theme: Git-Wiki Default


This is an example of layout built using default git-wiki theme

한글 폰트1234
To use it as your default theme you've to change layout configuration in your _config.yml, for example:

```
defaults:
 -
    scope:
      path: ""
    values:
      layout: "git-wiki-default"
 -
    scope:
      path: ""
      type: "pages"
    values:
      layout: "git-wiki-default"
```

**NOTE:** it's the default configuration available in _config.yml

