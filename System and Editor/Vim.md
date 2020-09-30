# Vim

|u | undo|
|-|-|  
|i|editor mode|
|ctrl-R | redo|
|set nu |     |
vim rc  

## clear all

```bash
gg
dG
```

## when exit

|cmd| description|  
|-|-|
|:q| to quit (short for :quit)|  
|:q!| to quit without saving (short for :quit!)|  
|:wq | to write and quit |
|:wq! | to write and quit even if file has only read permission (if file does not have write permission: force write) |
|:x | to write and quit (similar to :wq, but only write if there are changes) |
|:exit | to write and exit (same as :x) |  
|:qa | to quit all (short for :quitall) |
|:cq | to quit without saving and make Vim return non-zero error (i.e. exit with error) |  