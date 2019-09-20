# Unleashing the power of the command line

This covers some of what Masa did before and then adds some.

## Log onto server

Reminder: our server runs on a Linux operating system. The following commands are to 'interact' with that operating system and do NOT work on Windows. Most will work exactly the same on a Mac.

    $ ssh username@fgcz-c-047.uzh.ch

## Unix commands and working with file types common in bioinformatics

By default, the server uses bash (Bourne Again SHell) to interpret your commands to tell the computer what to do. 

### Pro tips for working (quickly) in the terminal

- At the prompt (where the `$` always is), the up/down arrows allow you to scroll through the commands you have run during the session. 
- To put your cursor at the beginning of the line while you're typing at the prompt: `Ctrl + a`
- To put your cursor at the end of the line while you're typing at the prompt: `Ctrl + e`
- While you're typing a file or directory that might be really long, you can type the first few characters, then press `tab` (the key that looks a bit like `-->|`) and it should auto-complete if you typed enough characters for it to figure it out. This is called tab completion. 
- To go to the last directory you were in (especially useful if you went from /omg/super/long/path to /here/is/another/long/path in one command):

     $ cd -
    
- Want multple windows/panes open at the same time? Try using iTerm2 (you have to download from the internet) or try using something called `tmux` (google it!). 

-------------------------

### `man` pages

These are (usually) good resources for the commands we will run. They include the arguments the unix command expects and what other options are available.

    $ man pwd
    $ man less
    
Once the man page is open, you can use `d` for page down and `u` for page up. Type `q` to exit page back to command line prompt.

Google the command and you will find numerous examples of how to use the command plus its various options to do the exact thing you want it to do. 

-------------------------

### Tab delimited files

The output of most bioinformatic software is really just a text file. With a lot of lines. For files containing certain types of infomation (raw sequencing reads -> FASTQ; mapped reads -> SAM/BAM; etc), there are standardized formats to which everyone adheres. Aside from having specific types on information stored on lines (ie FASTQ, every 4th line has the same type of information), many files are output as tab delimited files, in which each column contains a specific type of information. Not only understanding how the files should be formatted, but also knowing how to extract the information you want from them via command line is a quick way to organize/check the output. The tab character is `\t` and newline is `\n`. 

-------------------------

### I/O stream redirection

1. You can manipulate a data stream using multiple commands on one line using the pipe `|`. This takes the standard output (usually what's printed on the screen) of the first command and immediately enters it as input for the following command. Spaces are not required surrounding the pipe, but it makes it easier to read. 

    $ command1 file.txt | command2
    
As hard as you may try, some series of commands are just not compatible. This should not really be the case for the course, but if you start trying to make the longest single command possible, it just won't work!

2. You can redirect standard output (most of what's printed to the screen) to a file and save using the `>` character.

    $ outputcommand input.txt > thisistheoutput.txt
    
Achtung! This redirection will overwrite any file of the same name every time you run the command. If you want to append to a file, you can use `>>`.

-------------------------

### More about `less`

The `less` command is a nice way to view and search any standard output printed to a screen. (The `cat` command prints all the contents of a file as standard output). I often use it to 'open' a file to see its contents. Once you see the contents on the screen, you are able to scroll through using the arrow keys, search for patterns, and more (check the `man` page!). This is really useful if a file format requires a special tool to view the file, but subsequently would print everything on the screen all at once with no hope of scrolling through or searching. Search further down the document using `/` then typing the pattern you want to look for, `?` to search for the pattern further up in the file, `n` to find the next instance of the pattern. Type `q` to exit back to the command line prompt.

    $ less file.txt

-------------------------
    
### Count stuff with `wc`

Counts and prints number of lines, words, and bytes (all three by default) for each file.

    -l counts only lines
    -c counts only bytes (normal ASCII characters are 1 byte)
    -m counts only characters
    
    $ wc -l file.txt
    $ wc -c file1.txt file2.txt
 
-------------------------
   
### Pattern grab with `grep`

The `grep` command searches for matches to a pattern of characters in each file and prints the entire line that contains the pattern. This is case sensitive by default.

    grep [-civ] "pattern" file(s).txt
    
    -c counts the number of lines it appears in, suppresses printing
    -v inverse match: returns the lines that do NOT contain the pattern
    -i Perform case insensitive match
    '^word' ^ searches for lines beginning only with 'word'
    'word$' $ searches for lines only ending with 'word'

Examples

    $ grep "CDS" Medtr.gff | less
    $ grep -c "CDS" Medtr.gff
    $ grep -vc "CDS" Medtr.gff

-------------------------

### Slice columns with `cut`

The `cut` command allows you to extract information from specific columns. Downside: you need to know the number(s) of the column(s) you want. Counting starts at 1 from the left most column.

    cut –f [-s]  1,4-6 [-d ","] file.txt

    -f Select fields (columns); Range or comma separated numbers
    -s Return only lines which contain one or more delimiter characters
    -d Field delimiter. Tab (\t) is default.

Examples

    $ cut –f 1,3-5,7 Medtr.gff | less
    $ cut –s Medtr.gff  | less

-------------------------

### Translate or delete with `tr`

Replaces characters (Char1) with other characters (Char2) and prints to screen. You can also do character/string replacement with something like `sed`, but that requires knowledge of regular expressions. I won't cover that here, but it's not difficult to use.

    tr [-d] Char1 [Char2]

    -d delete Char1 (no Char2), in single quotes

Reminder: New line = `\n`, tab = `\t`, and space = ' '

Examples

    $ echo "Hello World" | tr ' ' '\n'
    $ cat Medtr.gff | tr '\t' ' ' | less
    $ cat MedtrChr2.fa | tr -d '\n' | less

-------------------------

### `sort` lines alphanumerically

    sort [-rnu] file
    -r Sort in reverse
    -n Use numerical sort
    -u Return only unique lines

Examples

    $ echo "10 1 12 11 100 2" | tr ' ' '\n' | sort
    $ echo "10 1 12 11 100 2" | tr ' ' '\n' | sort -n
    $ echo "10 1 12 11 100 2" | tr ' ' '\n' | sort -nr
    $ echo "12 10 11 12 12 11" | tr ' ' '\n' | sort -u

-------------------------

### Feel special with `uniq`

Find unique instances of strings.

    uniq [-c] file
     -c Count the number of occurrences for each output line: 1st column is the count, 2nd column is the unique string

Examples

    $ echo "12 10 11 12 12 11" | tr ' ' '\n' | uniq
    $ echo "12 10 11 12 12 11" | tr ' ' '\n' | sort | uniq
    $ echo "12 10 11 12 12 11" | tr ' ' '\n' | sort | uniq -c

Considers only *consecutive* duplicates. Therefore, you almost always want to use sort first!

-------------------------

### A word on working with compressed files

The commands above work with uncompressed, plain text files. Many tools either output compressed (gzipped, bzipped, etc) files, collaborators will send you compressed files or you should compress your files to save disk space. When they are compressed, you need to slightly modify your commands to deal with those files. Some are pre-modified:

    $ zless file.txt.gz
    $ zcat file.txt.gz
    $ zdiff file.txt.gz
    $ zgrep file.txt.gz
    
These are special. Unfortunately, you can't just put a 'z' in front of any command to have it magically work!

Sometimes, you'll have to pipe commands to make it work: 

    $ zcat file.txt.gz | cut -f1 -d "_" > newfile.txt
 
-------------------------
   
### Command cheat sheet

{insert table here}

# Exercises

GFF file located in /srv/kenlab/bio373_2019/SNPdata
1. GFF files contains information on features of a sequence: genes, introns, etc. Take a look and familiarize yourself with the format.
2. How many lines are in Medtr.gff? Characters?
3. How would you get only the ID information from the attribute column (column 9) of Medtr.gff without printing the header line?
4. How many of each type of feature (column 3) are there?
5. How many CDS’s are there within chr1?
