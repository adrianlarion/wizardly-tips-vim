# HOW TO READ:
`<C-g>` - means Ctrl+G
`g<C-g>` - means press g then press Ctrl+g
`<S-g>` - means press SHift+g

##### Mixed



```find ~ -size +10M | vim -```




* find files and open the results in vim. While in vim you can press `<gf>` to open a file (from the results provided by find) in vim. 
- HINT: if your file has spaces vim will complain that the file is not in the path. This happens because it truncates the file when a space appears, thus trying to reach the file at an erroneous path. So instead use `<S-V>` to select the hole line and then press `<gf>` 


* The `<C-g>` magic - it displays  file name and position. `<g><C-g>` will display number of words, num of lines and other useful stats.


##### g command
* `gj`, `gk` - move up and down by display lines (whereas j and k move by real lines (if you have a wrapped line then you'll see))
* `g0` first char of display line
* `g^` first nonblank char of display line
* `g$` end of display line
* `ge` end of prev word (use in conjuction with `e`)
* `gU` uppercase
* `gu` lowercase
* `g~` swap case
* `{N}gt` -switch to tab page number n
* `gt` next tab
* `gT` prev tab
* `gf` go to word under cursor
* `g;`traverse forward through the change list
* `g,`traverse backward through the change list
* `gi` go back to last insert position and enter insert mode, from wherever you are
* `gf` go to file under cursor **NICE**
* `ga` - show char codes for char under cursor
* `gn` - char wise visual mode and select next search match (after a  `/` search)
* `gN` - char wise visual mode and select prevsearch match
* `g&` repeat last `:s` on all lines
* `gE` go backward to end of previous WORD
* `ge` go backward to end of previous word
* `g_` go to the last char N -i lines lower
* `gd` go to definition ofword under cursor
* `gg` cursor to line N , def first line
* `gm` go to char in themiddle of the screen line
* `gM` go to char in themiddle of the text line
* `go` go to byte N in the buffer (char starting  from beginning of  file)






