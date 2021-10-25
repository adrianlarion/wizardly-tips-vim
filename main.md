* find files and open the results in vim. While in vim you can press <gf> to open a file (from the results provided by find) in vim. 
- HINT: if your file has spaces vim will complain that the file is not in the path. This happens because it truncates the file when a space appears, thus trying to reach the file at an erroneous path. So instead use <S-V> to select the hole line and then press <gf> 
`find ~ -size +10M | vim -`
