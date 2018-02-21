## Create new file in Explorer mode

hit to `%`.


## Format JSON with jq

```
:%!jq '.'
```

or 

```
:.w !jq
```

reference: https://stackoverflow.com/a/7025184

See `:help :w_c`


## Delete the text between matching XML tags

reference: https://stackoverflow.com/a/946241

before:

`<tag> content </tag>`

`:dit`   (`it` is for "inner tag block")

after:

`<tag></tag>`
