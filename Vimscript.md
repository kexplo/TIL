# Vimscript

## augroup

`:autocmd` adds to the list of autocommands regardless of whether they are
already present.  When your .vimrc file is sourced twice, the autocommands
will appear twice.  To avoid this, define your autocommands in a group, so
that you can easily clear them: >

```vimscript
augroup vimrc
  " Remove all vimrc autocommands
    autocmd!
  au BufNewFile,BufRead *.html so <sfile>:h/html.vim
augroup END
```

```vimscript
augroup vimrc
    autocmd!
augroup END

" ...

" Put autocmd where you want
au vimrc BufNewFile,BufRead *.html so <sfile>:h/html.vim
```

## autocmd

`:au[tocmd] [group] {event} {pat} [nested] {cmd}`

Add {cmd} to the list of commands that Vim will execute automatically on {event} for a file matching {pat} |autocmd-patterns|.

Note: A quote character is seen as argument to the :autocmd and won't start a comment.

Vim always adds the {cmd} after existing autocommands, so that the autocommands execute in the order in which they were given.  See |autocmd-nested| for [nested].

```vimscript
:au BufNewFile,BufRead *.html so <sfile>:h/html.vim
```

## autocmd!

`:au[tocmd]! [group]` : Remove ALL autocommands.

Note: a quote will be seen as argument to the :autocmd and won't start a comment.
Warning: You should normally not do this without a group, it breaks plugins, syntax highlighting, etc.

When the [group] argument is not given, Vim uses the current group (as defined with ":augroup"); otherwise, Vim uses the group defined with [group].

## function

`:fu[nction][!] {name}([arguments]) [range] [abort] [dict] [closure]`

The `name` must be made of alphanumeric characters and '_', and must start with a capital or "s:".  Note that using "b:" or "g:" is not allowed.

When a function by this name already exists and [!] is not used an error message is given.  There is one exception: When sourcing a script again, a function that was previously defined in that script will be silently replaced.

When [!] is used, an existing function is silently replaced.  Unless it is currently being executed, that is an error.

NOTE: Use ! wisely.  If used without care it can cause an existing function to be replaced unexpectedly, which is hard to debug.

```vimscript
function! Bar()
  echo "in Bar"
  return 4710
endfunction
```

## References

`:help autocmd` on Vim
