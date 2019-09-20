# Unleashing the power of the command line

This covers some of what Masa did before and then adds some.

## Log onto server

Reminder: our server runs on a Linux operating system. The following commands are to 'interact' with that operating system and do NOT work on Windows. Most will work exactly the same on a Mac.

    $ ssh username@fgcz-c-047.uzh.ch

## Unix commands and working with file types common in bioinformatics

By default, the server uses bash (Bourne Again SHell) to interpret your commands to tell the computer what to do. 

### `man` pages

These are (usually) good resources for the commands we will run. They include the arguments the unix command expects and what other options are available.

    $ man pwd
    $ man less
    
Once the man page is open, you can use `d` for page down and `u` for page up. Type `q` to exit page back to command line prompt.

Google the command and you will find numerous examples of how to use the command plus its various options to do the exact thing you want it to do. 

### Tab delimited files

The output of most bioinformatic software is really just a text file. With a lot of lines. For files containing certain types of infomation (raw sequencing reads -> FASTQ; mapped reads -> SAM/BAM; etc), there are standardized formats to which everyone adheres. Aside from having specific types on information stored on lines (ie FASTQ, every 4th line has the same type of information), many files are output as tab delimited files, in which each column contains a specific type of information. Not only understanding how the files should be formatted, but also knowing how to extract the information you want from them via command line is a quick way to organize/check the output. The tab character is `\t` and newline is `\n`. 

### Pipe dreams

You can manipulate a data stream using multiple commands on one line using the pipe `|`. This takes the standard output (usually what's printed on the screen) of the first command and immediately enters it as input for the following command. Spaces are not required surrounding the pipe, but it makes it easier to read. 

    $ command1 file.txt | command 2
    
### More about `less`

The `less` command is a nice way to view and search any standard output printed to a screen. I often use it to 'open' a file to see its contents. Once you see the contents on the screen, you are able to scroll through using the arrow keys, search for patterns, and more (check the `man` page!). This is really useful if a file format requires a special tool to view the file, but subsequently would print everything on the screen all at once with no hope of scrolling through or searching. Search further down the document using `/` then typing the pattern you want to look for, `?` to search for the pattern further up in the file, `n` to find the next instance of the pattern. Type `q` to exit back to the command line prompt.

    $ less file.txt
    
### Count stuff with `wc`

Counts and prints number of lines, words, and bytes (all three by default) for each file.

    -l counts only lines
    -c counts only bytes (normal ASCII characters are 1 byte)
    -m counts only characters
    
    $ wc -l file.txt
    $ wc -c file1.txt file2.txt
    
### Pattern search with `grep`

Searches for matches to a pattern of characters in each file and prints the entire line which contains the pattern. This is case sensitive by default.

    grep [-civ] "pattern" file(s).txt
    
    -c counts the number of lines it appears in, suppresses printing
    -v inverse match: returns the lines that do NOT contain the pattern
    -i Perform case insensitive match
    '^word' ^ searches for lines beginning only with 'word'
    'word$' $ searches for lines only ending with 'word'

Examples

    $ grep "CDS" Medtr.gff | less
    $ grep –c "CDS" Medtr.gff
    $ grep –vc "CDS" Medtr.gff

