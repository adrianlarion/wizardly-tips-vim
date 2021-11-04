# Wizardly Tips Vim
A fast way to benefit from vim's power. Ideally you should read a good book and the `:h` section. But if you're in a hurry this should serve you well.
This is a list of somewhat unknown but highly useful Vim tips and tricks. I try and keeep the list succint by not showing well known or less useful commands.







# HOW TO READ SHORTCUTS:



`<Esc>` - Escape. Usually you should press the key, not type it. When you see `ii<Esc>` you type ii and then Escape, not 'ii<Esc>` literally.




`<C-g>` - means Ctrl+G




`g<C-g>` - means press g then press Ctrl+g




`<S-g>` - means press SHift+g




`<C-Z>` - means Ctrl+Shift+Z (Shift is not shown but implied since we have an uppercase Z)




`<zZ>` - z followed by Shift+Z





`<C-m><CR>` - type Ctrl+M followed by Enter. `<CR>` is Carriage Return (another word for Enter key)







### Find files and send them to vim.
```find ~ -size +10M | vim -```



* find files and open the results in vim. While in vim you can press `gf` to open a file (from the results provided by find) in vim. 
- HINT: if your file has spaces vim will complain that the file is not in the path. This happens because it truncates the file when a space appears, thus trying to reach the file at an erroneous path. So instead use `<S-v>` to select the hole line and then press `gf` 
- HINT2 - you can use any other command that outputs to stdout and send it to vim. 
- HINT3 - you can open the file under cursor in a new window with `<C-w>f`



### Encrypt
* You decide to write your secret journal using vim. Trouble is - anyone can read your unencrypted text files. Or can they? By using `vim -x mysecretfile.txt` you use encryption on file. You'll be asked for a key (password) to encrypt/decrypt your file. Warning - if you forget your key bye bye file. 
- HINT - you can also encrypt a file after you open it by typing `:X`




### Macros
* I'm still amazed by how many vim users totally ignore the power of register playback. That is - you record your actions into a register and play those actions back. It's a bit weird to wrap your head around it but once you do - you'll be amazed. 
Here's how it works
- press `qa` to start recording your actions into register `a`
- Perform some actions that you want repeated. Here's a very useful example for coders - custom line by line editing. Let's say that you need to go line by line, search for the word "foo" and place it in tags (<foo>) and place those tags again on the same line at the end of the line.
- This is a pretty complex scenario. Doing it by hand for thousands of lines it's PITA (Pain In The A*s). What can you do? Write an awk script? A Python script? NOOOOO. You use vim register playback.
- So you start your action by implementing a movement that will go to the next line or the next pattern. This is very important because it will make the playback "smart", as it will go on the next line or pattern and do the work.
- In your case you would start by typing  `/foo` and pressing `<CR>` (Enter). This takes care of always finding the next pattern and working on it.
- No do your desired operations. `i<<Esc>ea>` which will insert the bracket, then to normal mode, go te end of the word, append the closing bracket. 
- next you copy the whole bracketed foo by pressing `ya>` (yank all in between brackets) followed by `$p` (go to end of the line and paste)
- Your work is done. You press `q` to stop recording. 
- time for PAYBACK ...ah, I meant PLAYBACK. 
- you press `@a` to playback the contents of register `a` (into which you recorded your actions). You watch amazed how vim repeats "smartly" the operations you just performed previously. 
- Now you can press `@@` to repeat the previous playback as many times as you want. Or you could press `10@@` to repeat the playback for 10 times. Or if you want this applied to your whole file type `:% norm @a` - which means - apply the macro to all lines.






### Open files in own tab/win
* Your girlfriend's father comes to visit you. You want to impress him by opening a bunch of text files, each in it's own window in vim (and show him that you're very smart and tech savvy)
- You write `vim -o f1 f2 f3 f4` to open the specified files with windows split horizontally. use `vim -O f1 f2 f3` to split them vertically. If you have lots of files and you dont want a hundred narrow windows use `vim -o5 *.txt` to open a max of 5 windows (horizontally stacked). 
- You could instead open them in tabs by using the same syntax `vim -p f1 f2` or `vim -p5 *.txt`



### Info
* The `<C-g>` magic - it displays  file name and position. `g<C-g>` will display number of words, num of lines and other useful stats.



### Enter special chars
* Enter digraphs (speciar chars). Use `<C-k>` in Insert mode and type the digraph code. For example you can enter the pound sign using 'Pd' (£). Type `:h  digraph-table` for more info. To just view the codes type `:dig`



### Quickly write buffer to disk
* `ZZ`  - write the buffer to disk if modified and exit. I bet you didn't knew that - but if you did BRAVO, you know how to quit Vim! 



### Sort
* `:%!sort -u` - sort all lines and remove duplicates. `%` refers to the range and it means all the lines. You could use `1,5` for example and it would only sort lines 1 to 5. The `!` in this context means - execute an external shell command (in our case sort) with the range as stdin, when you're done put back in place the stdout from that command.




### Paste register
* While in insert mode press `<C-r>` followed by a registry, such as `a` or `b` or `@` or `:` or `+`. eg: `<C-r>a`. This will insert the contents of the registry at cursor.



### Insert mode navig
* While in insert mode press `<C-h>` to backspace a char, `<C-w>` to delete a word backwards, `<C-u>` to delete until the beginning of the line (all without leaving Insert mode)



### Autocomplete
* Did you knew you also have autocomplete by default? While in insert mode press `<C-n>` or `<C-p>` to cycle through possible autocompletions. 




### How to quit Vim
* There are lots of famous memes about people not knowing how to quit Vim. Here's a little secret - you can quit the current buffer(if you opened just a single file) without writting to disk by pressing `ZQ`.





### Repeat Insert
* Everyone knows that you can insert a character repeatedly using a command such as `20i-<Esc>` - which will insert 20 '-'. Or you could do something like `10aHello World Of Vim.<Esc>`. This will do 10 appends of the phrase 'Hello World Of Vim'



### On the fly computations
* You're deeply immersed into writting some cool code for your amazing program. Suddenly you have a hankering for some on the fly computations. You want to ... no, you **need** to calculate 42 multiplied by 42. Well damn. You have to stop your editing, pull up the calculator, do the calculations, copy them, go back to editing, and paste them. PHEW!!!
- Too much work.
- Here's a simpler solution. While in Insert mode press `<C-r>=`. You will see an equal sign appearing in the bottom of the window. Enter your math formula - eg: `42*42` and press enter. The result will be magically inserted before cursor while still keeping you in insert mode. (the result is 1764 if you were curious).
- Note of warning, certain operations such as `2^10` or `2**10` which you would expect to raise 2 to power of 10 don't work in that syntax. You'll have to use function instead, the `pow(2,10)` will raise 2 to power of 10.
- You can see more info by checking the help `:h expression`



### History of past searches
* You search for various keywords in your code. After a while you want to search for some of those keywords again but you don't remember them exactly. No worries. Just press `q/` and you'll open a small window at the bottom containing your searches. Navigate with `jk` and press <CR> (Enter) to repeat the selected search. Or just close that window with `:q`


##### Useful selection of g commands
* `gf` open file under cursor
* `g;`traverse forward through the change list (for example after you edit in Insert mode)
* `g,`traverse backward through the change list
* `gi` go back to last insert position and enter insert mode, from wherever you are
* `gf` go to file under cursor (or select the whole line with `<C-v>` if the path has spaces
* `ga` - show char codes for char under cursor
* `g&` repeat last `:s` on all lines (last replace command)
* `gv` repeat last visual selection.


##### Paste word under cursor
* You're in command  mode, trying to write a complex search & replace command. You want to search for the word "Supercalifragilisticexpialidocious" and replace it with "Super". Your fingers go on strike before you even start. What to do?. Well, assuming your cursor is on the said word you can paste that word in  mode by typing `<C-r><C-w>`. This will paste the word under the cursor. 
- this is also useful when you have words with tricky spelling.


### Create and use abbreviation.
* You're documenting your code. It's a mess. You need to use separators to make things clearer. You decide to use a long string, like `#--------------------------------------------------`. Typing it every time it's cumbersome and you will probably not get the amount of dashes right every time. 
What to do?
You could yank it in a register and paste it. But I think there's another, more flexible way.
You use abbreviations. Type:




`ab sep #-----------------------------------------------<CR>`




now in Insert mode type `sep` and press `Tab`. Your long separator is inserted. 

- HINT - to clear abbreviations type `:abc`



### List words under cursor.
* You're hard at work debugging your code. You left clues in form of comments containing the word "debug" - eg "#debug this or you'll get fired". You're somewher in the middle of the file but how many more lines containing the word "debug" are there?
- with the cursor over the word "debug" (or any other word) from Normal mode type `]I`. A list will appear at the bottom containing all lines containing the word under cursor, starting from the current line position. If you want the search to start from the beginning of the file (instead of curent line) type `[I`
- you can use this cool function for quickly displaying lines containing error codes, certain strings, etc. 


### Pattern searching tricks
* `/debug/1` - goes to the line below the line containing the pattern "debug". use another number for a negative offset or a larger offset. eg: `/debug/-4` -  4 lines above the line containing the debug pattern.
* `/debug/e+1` - go to +1 char after the end of pattern "debug". eg: "debug3" will put the cursor on "3"
* `/debug/b+3` - go to +3 chars after the beginning of pattern debug. In this case the cursor will be on "u" (not 'b' because counting starts at 0, where 0 char is the first char, in this case 'd')
* If later you decide you need another offset use `//e+10` - where 'e+10' is your new offset but using the old seach pattern.


### Use expression register to store and iterate
* You have a piece of code with the following word repeated 10 times: `var a1=42`. You want to have `var a1=42`, `var a2=42`, etc until `var a10=42`. How could you do that? By hand? NOOO.
* First set a variable to 0. `:let i=0`
* Now start to record a macro `qa` (record in register "a")
* Find your desired value using `/\va1/e`. The `/e` tells vim to place the cursor at the end of the patter, in this case on the digit `1`. Note that we're using `/\v` to signal vim to use 'very magic' (which means we don't have to escape certain special chars)
* Now for the magic. Start by incrementing the variable i. `:let i += 1` (spaces are important)
* press `s` to enter insert mode and delete the digit `1` at the same time.
* And now insert the value of `i` using the expression register. `<C-r>=i` (which will just echo the value of i and place it at cursor)
* Stop recording with `q`
* Apply the macro to all lines using `% norm @a` (on all lines eecute macro stored in 'a' register)
* DONE.


### Save and load your sessions.
* You are immersed into a highly complex editing session. You have log files, scripts, documentation and other files, all open in windows and tabs. Suddenly there's a computer crash. You open vim again but now you have to re-open all those files by hand (and place them in their various locations and windows). That sucks. 
* Instead you can simply save your session. Type `:mks mysession` (where myssesion is your session file) to save your session to a file. Next time you boot up vim use `vim -S mysession` to load your session.
* Or do it directly from vim. `:so mysession`



### Go back in history.
* You edit a bunch of files. After a couple of months you need to change  them again. Trouble is, you don't remember exactly what files you edited. 
* Type `:ol` and vim will show you your history (of file edits).
* To open one of these files type `:bro :ol` and choose a number coresponding to the desired file.



#####  Go back to the line you last edited
* You start editing a file. Suddenly the phone calls. Your cat has won the lottery. After you collect the prize and buy the cat some toys you come back. You want to resume your work but you don't remember the exact line you were in. The file name it's also vague. What do you do?
* You type "`0" (backtick followed by zero). Vim will open the last edited file on the exact line you left it.



##### Terminal inside vim
* You have 2 scripts and a config file open in separate windows. It's all good. But you need to run some quick commands in the terminal. Oh no. You don't want to leave the warm bossom of Vim. What to do?
* You could press `<C-z>` and put Vim in background (which will open a terminal). Then you could type `fg` in terminal to get back to vim.
* Or you could type `:sh` to open a shell temporarily and hide vim. When you're done you could press `exit` and get back to vim.
* But all these options aren't exactly what you want. You want the terminal, true, but you also need to view and edit your files **at the same time**.
* You type `:ter` and a terminal opens alongside your open windows. Yay! When you're done just type `exit` in the terminal.


#####  Persistent undo
* Wouldn't it be cool to be able to undo your changes **after** you reopen your file? In vim this is quite easy to do. 
* Start by creating a dir where you want your undo info to go. Here's a good choice `~/.vim/undo-dir`
* Add the following two lines to your `.vimrc` (located usually at `~/.vimrc`:
```
set undodir=~/.vim/undo-dir
set undofile
```

### Load the previous  command
* You type a complex  command. Later you want that command again. One option would be to use the arrow keys and cycle through history. That works but takes your hands from the home row position. 
* Another way to load last  command is `:<C-r>:`. That is: start by tyying colon, press Ctrl+R and type colon again. Colon is a register storing your last  command. Ctrl+R outputs the contetns of a register.


### Append your registers.
* You open two important  documents. From one you start yanking important lines in a register. You go to the second document and paste but realize you missed a couple of lines. Damn. Now you have to yank all the lines and the ones you missed.
* Or do you? Instead you can just append to the register in which you yanked. Simply yank in the uppercase version of the register and you will append (eg: if you yanked in `a` now you append by yanking in `A`


### Range shortcut
* Press `5:` and vim will type for you in the  command line `:.,.+4` 


### Visual Block Syntax
* If you've used Vim for a while you know you can edit multiple lines at a time. Use Visual Block `<C-v>`, use a motion `3j`, insert some text `IHelloWorld` and press `<Esc>`. BAM! Your inserted text is typed on all lines. 
* But here's the real trick which few people know. Let's say that your vertical line spans some short lines. Eg:
```
I'm a long line, very vory long, YEAH.
Shory.
I'm a long line, very vory long, YEAH.
```
* You start the visual block let' say on the word 'YEAH' and it spans the whole 3 lines. If you use `I` and insert text the short line will remain empty (where it was empty space it'll still be empty space). But if you use `A` (append) then you'll also type text in the empty line. Pretty neat, eh? 
* In order to remember think that `A` Append always writes text. 


### Quickly run an external command
* It's cool to be able to run external terminal commands with `:%!sort` which passes all lines to sort and writes back the output. But there's a neat trick that will save you some time:
* `!5G` - will start writting an  command that looks like `:.,+4!`. You then will type your desired program `sort` and press enter. You start with `!` and then specify a motion from the current line `5G`. You can use other ranges, like `!4j` which will sort the following 4 lines from your current line.
* An even faster shortcut is `!!`. This will pass the current line to the external program (and replace it with the output). A known use case of this is to quickly insert a timestamp into your document `!!date`


### Replace with confirmation
* You have document. In it you have a variable called "money". You want to rename it to "cash" but only for certain cases. In that case you should type: `:%s/\v<money>/cash/gc`.  This long winded command uses very magic `\v` so that you won't have to escape `<>`. Those chars mark the beginning and end of a word. We use them because you presumably want to replace only the instance of "money" and not "zamoney" or "moneys".  Finally the `c` flag will prompt a confirmaton dialogue each time an entry is found.
* HINT - use `<C-e>` and `<C-y>` to scroll up and down so you can better see the context during the replace confirm dialogue.


### Documentation at your fingertips
* You're editing a bash file. You read the output from `find` and for each result you want to run an external script. You decide to use `xargs` instead of `-exec {} \;`. But as you write your command you find that you're rusty on xargs arguments. What to do?
* You could put vim in foreground `<C-z>`, `man xargs`, `fg`. Or you could open a terminal inside vim with `:ter`, check the docs then close the window with `<C-w><C-c>`. 
* But there are 2 better ways to do this.
* Just press `K` (Shift+k) on the keyword you're interested in and Vim will open the relevant man page. If it's another type of file (not bash) vim will also try to find the help for that particular keyword. For example you could put your cursor on `pow()` in a python file, type `K` and vim will show you the docs. Pretty cool, eh? One keystroke and the docs are open! AWESOME!
* But there's more. There is a filetype plugin which gives you an enhanced version of man. First install it with `:runtime! ftplugin/man.vim` (put it in your .vimrc if you want it open at startup).
* Now type `:Man xargs`. Vim will open the manual page inside vim in a separate window. That's cool and it save you the hassle of opening a terminal by hand and typing man inside it. 
* That's cool but it's just half of the gooodness. The other half is this. Inside this man window put your curosr on a keyword. Now press either `\K` (backward slash followed by Shift+k) or `<C-]>`. If there exists a man for that keyword it will open inside the same page. Like a browser. THis functionality is NOT present in the default man pages. You can navigate back and forth with `<C-o>`, `<C-i>`
* And for a last tip - you can use `\K` inside vim too on a keyword. Then vim will open the new and improved man page instead of the default documentation (whith opens with `K`)
* BUT NOTE! the `:Man` command works only for man pages for linux, not other programming languages. That is if you press `\K` on 'pow' inside a python file you will get the man page for 'POW(3)' (linux programmer manual) and not python.


### Input/output with external commands.
1. `:!date` - execute date command and print the results 
2. `:r !date` - execute date command and append the output after range last line. If no range provided use current line. Thus the results from date will be appended after the current line in our case.
3. `:w !date` - send all the lines as stdin to date command. Print the results (no insertion into current buffer)
4. `:1,3!date` send lines 1 to 3 as stdin to date command. Replace the lines in range (1,3) with output from date.



### Put each word on a new line
* Put all words from a line on a new line. eg: "hello world of vim" will be: "hello\n world\n of\n vim\n". How? With `:.!xargs -n1`. It says: send current line as stdin to xargs. You, xargs, split the line into args separated by blank space and `echo` at most one word. Then put the xargs output in place of the line. Since the output from xargs contains newlines this creates extra lines.
* You could also use a range, to put each word on the lines in range on a new line with `:1,5!xargs -n1`
* NOTE - xargs separates words by spaces. Vim has a default different way of separating words which may or may not corespond with xargs. (see `:h isk`


### Buffer delete
* As you know buffers are usually contents of files loaded in memory, with extra info attached to them (marks, registers, etc). 
* Let's say you decide to delete some buffers with `:bdel myverylongfile`. 5 minutes later you need again the buffer you deleted. Darn. What to do? You could try and remember the filename and reopen it. But what if there were lots of buffers you deleted and they havo complicated names?
* Vim it's here to help you. Just type `:ls!` to show all buffers, even [u]nlisted ones. You'll see your deleted buffer listed (they'll have a 'u' indicator in front of them for unlisted). Turns out deleting a buffer makes it unlisted. 
* If you really, really want to delete a buffer use `:bwipe`. 


### Autocomplete with files/lines
* You're editing a file. You have separator comments for sections, like `#DEBUG---------------------`. At some time you would like to insert that line into your current text. You could use a register but what if you have lots of types of separator comments (DEBUG, WIP, STABLE,etc)? Here's a better way:
* Type `<C-x><C-l>` (Ctrl+x, Crl+l). The autocomplete will show you whole lines and you can choose your separator comment line. (in Insert mode)
* <C-x><C-f>  will show you autocomplete options from the files in current directory. Quite cool.  (in Insert mode)
* After the autocomplete list appears you can cycle through it with `<C-n>` or `<C-p>`. 
* But here's an even crazier thingie - you can start typing an existing path and Vim will try and autocomplete it. Eg: you have typed `/` and are still in Insert mode. Type `<C-x>,<C-f>` and a list of dirs under root dir will appear. Cycle through them with `<C-n>` or `<C-p>`. If you want to go deeper you can't press Space because that will just enter that dir/file and a space afterwards. INstead press `<C-x>,<C-f>` and you will have inserted that dir/file name and will go one level deeper (to the next forward  slash) where you can cycle through the next options.
* You can use the above feature in combination with `gf` or `<C-w>f`  to open the files under cursor in vim. Very flexible.


### Repeat your insert
* You start editing a file. You type rather long text containing a copyright comment. You enter Normal mode, you jump to the end of the file and see that you will have to enter the same line again (for some reason). 
* Enter insert mode and press `<C-a>` - it'll insert the text you previously typed in Insert mode. 

### Better wrap
* After you set up the wrap option `:se wrap` you notice that words may be cut of in the middle at end of screen. Like so:
```
hello wo
rld of v
im
```
* Use `:se linebreak` and have the words not cut of in the middle:
```
hello 
world of 
vim
```

### Indent in Insert mode
* You're probably using the indent operators in Normal mode (to shift your lines to the rigt/left an amount of space). `>>` and `<<` will shift left/right a line. `>` works with other motion and text objects: `>ap`, `>3j`, etc. (`gg>G` will shift right all lines in a file)
* But if you're in Insert mode it can be a pain to switch to Normal and back again.
* It turns out you don't have to. You can use `<C-d>` and `<C-t>` to shift left/right right from Insert mode.


### Smart folding
* Folding is great. Trouble is - it sucks doing it by hand. Wouldn't it be great of Vim automatically created folds based on the indent level? That would work nicely for most programming languages.
* `:se foldmethod=indent` (instead of manual) and vim will automaticall fold based on 'shiftwidth' indents (if you change this setting in the middle of editing folding won't behave as expected)
* Now use the usual shortcuts to manipulate folds (`<C-z>,<C-o>,<C-m>,<C-r>,etc`)


### Increment numbers
* You're editing a file where you have years as dates. eg: "in 1825 the world...after 1841 something...". You receive a call from your editor/boss and he tells you that the dates are wrong. All years should be modified by adding 153 years to them. 
* Damn. You're in no mood for mental math juggle. And you have lots of years. What to do?
* Put your cursor over a year. Type `153<C-a>` (153 followed by Ctrl+a). Magically the year will be incremented by 153. Pretty cool. But you don't want to do this for all the years - you have hundreds of such dates. 
* Use a macro. `ggqa`. Go to top of file, start recording. `/\<\d\d\d\d\>` and press Enter (to find years). `153<C-a>` (153 followed by Ctrl+a). `q` to stop recording. `:%norm @a` to apply this macro to all lines.


### Copy a protected file and edit it immediately.
* You need to take a look and modify the dmesg file. Trouble is you can't if you started vim as a regular user - dmesg is owned by root. You could exit vim, make a copy, come back and re-open. 
* Here's a one liner that will save you the hassle: `:exec '!cp /var/log/dmesg .' | e dmesg`. First it executes the copy command. The pipe operator (vertical bar) chains commands in vim  mode. Immediately after a edit command is issued.


### Comment a whole file (or a portion)
* You open a long python script. You run it but you get errors. Darn. You decide to debug it by commenting it completely and uncomment sections as you see fit. 
* Commeting by hand it's troublesome - there are 1520 lines. You could try some fancy stuff with a Visual Block and insert but that might not be the best way.
* Instead you do `:%norm i#`. Kaboom - your whole script is commented out. Not smartly mind you (existing comments will be double commented) but it works nicely for your purposes. You can do the same for a .c file (or another file that supports these types of comments) with `:%norm i//`. You tell vim to execute a normal command across all lines (go to first char in line, even if it's empty and insert a comment)
* If you want to comment a section just use  range `:1,50norm i#`. If you have a visual selection and you type the colon you'll be presented with `:'<,'>` which means a range between the start and the end of the visual selection. Then you just type `norm i#` after the already present command.
* Using the same logic you could append semicolons to all lines in a file (or a range). Check it out: `%norm A;` - on all lines execute normal command A (which appends after the last char) and type a semicolon.

### Keep temp  commands in registers
* You have a particularly useful  command which you use repeatedly. It is: `:-1,+1norm i#` - it comments out the current line, the line above and the line below. As you work you want to execute this command, now an then. But you don't like typing it out every time.
* So instead you type the command in the text, just without the semicolon, on a new line: 
```
#bla bla
-1,+1norm i#
```
* Put your cursor on the line where the command is and delete it in a register, say 'w'. `"wdd` (in register w store this line and delete it from text)
* Now your register has your  command. But how to run it? 
* Easy. Type `:@w` - where 'w' is your register. Press enter and you'll execute this command. 
* Here's the explanation. When typing  commands a `@` sign in front of a letter signifies a register. the `@w` signifies the `w` register. When you type this special variable in  command window it just reads the contents of this register and executes it (after you press Enter).  
* Fun fact. You can repeat your last  command with `:@:`. The register `:` stores the last command. `@:` is used to signify this register while entering a command (and pressing Enter afterwards so that the command executes)




### Open extra files with terminal commands like find
* When you open vim with some files as arguments they are stored in `:args`. args are different from buffers. Buffers are all the buffers opened in memory and not necesarilly files. Args are the actual file args passed at startup. If you change the args the buffers stay untouched and viceversa. 
* Think about `:args` as a temporary list of files which you can modify to your desire. You navigate files in args with `:next` and `:prev` (whereas you navigate buffers with `:bn` `:bp` `:bf` `:bl`). 
* After you load vim and start editing some file you realize you have to also view some log files. What to do? 
* One possible way is to create a new buffer with `:new`. Afterwards put into this buffer all the results from find with `:r !find /var/log -size +1M`. All the results will be placed in the new buffer, each file path on a new line. Just press `gf` on a line and vim will open the file in a new window (and buffer). That's one approach.
* Another is to just run 
```
:args `find /var/log -size +1M -name '*.py' \|\| true`
```
(OR)
```
:args `find /var/log -size +1M -name '*.py' \| xargs -n1`
```
* Now if you type `:args` you'll see it is populated by all the results from find. Now you can naviate quickly throug these files. 
* Each method has drawbacks. Using this last approach you load all the results at once but the command is more tricky to type and your buffers may be overloaded. Using the first approach on the other hand will require you to go back to that new buffer and `gf` on each file you want opened. Choose what suits you best.
* For more see `:h backtick-expansion`



### Format with care
* You probably now about formatting with `gq`. You do things like `gggqG` to forma the whole file.
* But `gq` will also move the cursor to the motion end range (eg; end of file). In order to format but keep the cursor where it is you use `gw` - just in the same way. Like `gggwG`.


###


### patterns
* You're editing some source code left to you by your predecesor. It's messy. That fellow used to abbreviate troublesome variables with 'dbg_', like 'dbg_count', 'dbg_class', etc. Just how many of these variables are there? 
* You can search for them with the substitute command. Shocking, I know. Check it out: `:%s/<dbg_//gni`.
* This means: on all lines search for `<dbg_` (word start followed by 'dbg_'), replace with nothing (you could put a '&' but it's not needed), and use the following flags: g for global, i for case insensitive and n for no substitution. The 'n' flag is the trick here. It just prints how many matches there are without actually performing substitution.


### Quickly edit and reload your .vimrc
* You decide to make Vim behave exactly how you want it. You'll need to edit your .vimrc but that's alright. 
* After you open vim you can open your .vimrc with `:e $MYVIMRC`. Easy! 
* You've done your modification but now you want to see the effects. Just type `:so %` (source all lines) with .vimrc open and vim will source the new .vimrc.
