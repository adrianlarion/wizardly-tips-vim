# Less Known But Useful Vim
A fast way to benefit from vim's power. Ideally you should read a good book and the `:h` section. But if you're in a hurry this should serve you well.
This is a list of somewhat unknown but highly useful Vim tips and tricks. I try and keeep the list succint by not showing well known or less useful commands.







# HOW TO READ SHORTCUTS:




`<C-g>` - means Ctrl+G




`g<C-g>` - means press g then press Ctrl+g




`<S-g>` - means press SHift+g




`<C-Z>` - means Ctrl+Shift+Z (Shift is not shown but implied since we have an uppercase Z)




`<zZ>` - z followed by Shift+Z





`<C-m><CR>` - type Ctrl+M followed by Enter. `<CR>` is Carriage Return (another word for Enter key)







###### Find files and send them to vim.
```find ~ -size +10M | vim -```



* find files and open the results in vim. While in vim you can press `<gf>` to open a file (from the results provided by find) in vim. 
- HINT: if your file has spaces vim will complain that the file is not in the path. This happens because it truncates the file when a space appears, thus trying to reach the file at an erroneous path. So instead use `<S-V>` to select the hole line and then press `<gf>` 
- HINT2 - you can use any other command that outputs to stdout and send it to vim. 



###### Encrypt
* You decide to write your secret journal using vim. Trouble is - anyone can read your unencrypted text files. Or can they? By using `vim -x mysecretfile.txt` you use encryption on file. You'll be asked for a key (password) to encrypt/decrypt your file. Warning - if you forget your key bye bye file. 
- HINT - you can also encrypt a file after you open it by typing `:X`




###### Macros
* I'm still amazed by how many vim users totally ignore the power of register playback. That is - you record your actions into a register and play those actions back. It's a bit weird to wrap your head around it but once you do - you'll be amazed. 
Here's how it works
- press `qa` to start recording your actions into register `a`
- Perform some actions that you want repeated. Here's a very useful example for coders - custom line by line editing. Let's say that you need to go line by line, search for the word "foo" and place it in tags (<foo>) and place those tags again on the same line at the end of the line.
- This is a pretty complex scenario. Doing it by hand for thousands of lines it's PITA (Pain In The A*s). What can you do? Write an awk script? A Python script? NOOOOO. You use vim register playback.
- So you start your action by implementing a movement that will go to the next line or the next pattern. This is very important because it will make the playback "smart", as it will go on teh next line or pattern and do the work.
- In your case you would start by typing  `/foo` and pressing `<CR>` (Enter). This takes care of always finding the next pattern and working on it.
- No do your desired operations. `i<<Esc>ea>` which will insert the bracket, then to normal mode, go te end of the word, append the closing bracket. 
- next you copy the whole bracketed foo by pressing `ya>` (yank all in between brackets) followed by `$p` (go to end of teh line and paste)
- Your work is done. You press `q` to stop recording. 
- time for PAYBACK B**CH (to quote Jesse Pinkman)...ah, I meant PLAYBACK. 
- you press `@a` to playback the contents of register `a` (into which you recorded your actions). You watch amazed how vim repeats "smartly" the operations you just performed previously. 
- Now you can press `@@` to repeat the previous playback as many times as you want. Or you could press `10@@` to repeat the playback for 10 times. Or if you want this applied to your whole file type `:% norm @a` - which means - apply the macro to all lines.






###### Open files in own tab/win
* Your girlfriend's father comes to visit you. You want to impress him by opening a bunch of text files, each in it's own window in vim (and show him that you're very smart and tech savvy)
- You write `vim -o f1 f2 f3 f4` to open the specified files with windows split horizontally. use `vim -O f1 f2 f3` to split them vertically. If you have lots of files and you dont want a hundred narrow windows use `vim -o5 *.txt` to open a max of 5 windows (horizontally stacked). 
- You colud instead open them in tabs by using the same syntax `vim -p f1 f2` or `vim -p5 *.txt`



###### Info
* The `<C-g>` magic - it displays  file name and position. `<g><C-g>` will display number of words, num of lines and other useful stats.



###### Enter special chars
* Enter digraphs (speciar chars). Use `<C-k>` in Insert mode and type the digraph code. For example you can enter the pound sign using 'Pd' (£). Type `:h  digraph-table` for more info. To just view the codes type `:dig`



###### Quickly write buffer to disk
* `<ZZ>`  - write the buffer to disk if modified and exit. I bet you didn't knew that - I didn't until very recently. 



###### Sort
* `:%!sort -u` - sort all lines and remove duplicates. `%` refers to the range and it means all the lines. You could use `1,5` for example and it would only sort lines 1 to 5. The `!` in this context means - execute an external shell command (in our case sort) with the range as stdin, when you're done put back in place the stdout from that command.




###### Paste register
* While in insert mode press `<C-r>` followed by a registry, such as `a` or `b` or `@` or `:` or `+`. eg: `<C-r>a`. This will insert the contents of the registry at cursor.



###### Insert mode navig
* While in insert mode press `<C-h>` to backspace a char, `<C-w>` to delete a word backwards, `<C-u>` to delete until teh beginning of the line (all without leaving Insert mode)



###### Autocomplete
*Did you knew you also have autocomplete by default? While in insert mode press `<C-n>` or `<C-p>` to cycle through possible autocompletions. 




###### How to quit Vim
* There are lots of famous memes about people not knowing how to quit Vim. Here's a little secret - you can quit the current buffer(if you opened just a single file) without writting to disk by pressing `<ZQ>`.





###### Repeat Insert
* Everyone knows that you can insert a character repeatedly using a command such as `20i-<Esc>` - which will insert 20 '-'. Or you could do something like `10aHello World Of Vim.<Esc>`. This will do 10 appends of teh phrase 'Hello World Of Vim'



###### On the fly computations
* You're deeply immersed into writting some cool code for your amazing program. Suddenly you have a hankering for some on the fly computations. You want to ... no, you **need** to calculate 42 multiplied by 42. Well sh*t. You have to stop your editing, pull up the calculator, do the calculations, copy them, go back to editing, and paste them. PHEW!!!
- Too much work.
- Here's a simpler solution. While in Insert mode press `<C-r>=`. You will see an equal sign appearing in the bottom of the window. Enter your math formula - eg: `42*42` and press enter. The result will be magically inserted before cursor while still keeping you in insert mode. (the result is 1764 if you were curious).
- Note of warning, certain operations such as `2^10` or `2**10` which you would expect to raise 2 to power of 10 don't work in that syntax. You'll have to use function instead, the `pow(2,10)` will raise 2 to power of 10.
- You can see more info by checking the help `:h expression`



###### History of past searches
* You search for various keywords in your code. After a while you want to search for some of those keywords again but you don't remember them exactly. No worries. Just press `q\` and you'll open a small window at the bottom containing your searches. Navigate with `jk` and press <CR> (Enter) to repeat the selected search. Or just close that window with `:q`


##### Useful selection of g commands
* `gf` open file under cursor
* `g;`traverse forward through the change list (for example after you edit in Insert mode)
* `g,`traverse backward through the change list
* `gi` go back to last insert position and enter insert mode, from wherever you are
* `gf` go to file under cursor (or select the whole line with `<C-V>` if the path has spaces
* `ga` - show char codes for char under cursor
* `g&` repeat last `:s` on all lines (last replace command)
* `gv` repeat last visual selection.


##### Paste word under cursor
* You're in ex mode, trying to write a complex search & replace command. You want to search for the word "Supercalifragilisticexpialidocious" and replace it with "Super". Your fingers go on strike before you even start. What to do?. Well, assuming your cursor is on the said word you can paste that word in ex mode by typing `<C-r><C-w>`. This will paste the word under the cursor. 
- this is also useful when you haver words with tricky spelling.


###### Create and use abbreviation.
* You're documenting your code. It's a mess. You need to use separators to make things clearer. You decide to use a long string, like `#--------------------------------------------------`. Typing it every time it's cumbersome and you will probably not get the amount of dashes right every time. 
What to do?
You could yank it in a register and paste it. But I think there's another, more flexible way.
You use abbreviations. Type:




`ab sep #-----------------------------------------------<CR>`




now in Insert mode type `sep` and press `<Tab>`. Your long separator is inserted. 

- HINT - to clear abbreviations type `:abc`



###### List words under cursor.
* You're hard at work debugging your code. You left clues in form of comments containing the word "debug" - eg "#debug this or you'll get fired". You're somewher in teh middle of the file but how many more lines containing the word "debug" are there?
- with the cursor over the word "debug" (or any other word) from Normal mode type `]I`. A list will appear at the bottom containing all lines containing the word under cursor, starting from the current line position. If you want the search to start from the beginning of the file (instead of curent line) type `[I`
- you can use this cool function for quickly displaying lines containing error codes, certain strings, etc. 


##### Pattern searching tricks
* `/debug/1` - goes to the line below the line containing teh pattern "debug". use another number for a negative offset or a larger offset. eg: `/debug/-4` -  4 lines above the line containing the debug pattern.
* `/debug/e+1` - go to +1 char after the end of pattern "debug". eg: "debug3" will put the cursor on "3"
* `/debug/b+3` - go to +3 chars after the beginning of pattern debug. In this case the cursor will be on "u" (not 'b' because counting starts at 0, where 0 char is the first char, in this case 'd')
* If later you decide you need another offset use `//e+10` - where 'e+10' is your new offset but using the old seach pattern.


#####  Use expression register to store and iterate
* You have a piece of code with the following word repeated 10 times: `var a1=42`. You want to have `var a1=42`, `var a2=42`, etc until `var a10=42`. How could you do that? By hand? NOOO.
* First set a variable to 0. `:let i=0`
* Now start to record a macro `qa` (record in register "a")
* Find your desired value using `/\va1/e`. The `/e` tells vim to place the cursor at the end of the patter, in this case on the digit `1`. Note that we're using `/\v` to signal vim to use 'very magic' (which means we don't have to escape certain special chars)
* Now for the magic. Start by incrementing teh variable i. `:let i += 1` (spaces are important)
* press `s` to enter insert mode and delete the digit `1` at the same time.
* And now insert the value of `i` using the expression register. `<C-r>=i` (which will just echo the value of i and place it at cursor)
* Stop recording with `q`
* Apply the macro to all lines using `% norm @a` (on all lines eecute macro stored in 'a' register)
* DONE.


#####  Save and load your sessions.
* You are immersed into a highly complex editing session. You have log files, scripts, documentation and other files, all open in windows and tabs. Suddenly there's a computer crash. You open vim again but now you have to re-open all those files by hand (and place them in their various locations and windows). That sucks. 
* Instead you can simply save your session. Type `:mks mysession` (where myssesion is your session file) to save your session to a file. Next time you boot up vim use `vim -S mysession` to load your session.
* Or do it directly from vim. `:so mysession`



#####  Go back in history.
* You edit a bunch of files. After a couple of months you need to change  them again. Trouble is, you don't remember exactly what files you edited. 
* Type `:ol` and vim will show you your history (of file edits).
* To open one of these files type `:bro :ol` and choose a number coresponding to the desired file.



#####  Go back to the line you last edited
* You start editing a file. Suddenly the phone calls. Your cat has won the lottery. After you collect the prize and buy the cat some toys you come back. You want to resume your work but you don't remember the exact line you were in. The file name it's also vague. What do you do?
* You type "'0" (backtick followed by zero). Vim will open the last edited file on the exact line you left it.
