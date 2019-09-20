# BASH scripting

Bash scripts are a nice, basic way to automate, record your commands, and start practicing reproducibility!

## Basics

A bash script is really just a series of bash commands and/or any other command that's been configured to run via the shell. They are executed in the order you put them in the script. The very first line should be the "shebang", telling the computer which interpreter to use (this is only really necessary if you want to use a different interpreter/version/whatever than the default, but put it in there anyway).

Shebang:

    #!/bin/bash
    
You need a text editor (discussed below) to write the script. For good practice and human readability, the file should be saved with the `.sh` extension.

After the shebang, any line that begins with a `#` is considered a comment and is ignored by the computer when you run the script. 

Scripts are run as follows:

    $ bash script.sh
    
### Text editors

Anything that will save your file as a plain text. There are several options on the server already (nano, emacs, vim). These are applications to be used in the terminal and the files are saved in the current directory you're in when you open the application. On the other hand, there are several options from GUIs (TextWrangler, XCode, TextEdit, etc) that you can use on the local computer. These feel more intuitive to people, but you must then copy the script onto the server (using `scp`) if you want to execute it there. Microsoft Word does not count!

### Variables

    varName="value"

- varName stores "value" for the duration of your shell session
- No spaces around =
- varName can have letters, numbers and underscores but cannot start with a number
- Retrieve the value by prepending variable name with $ (ie $varName)
- $ can be used with curly braces or double quotes to avoid confusion with any following text (ie ${varName})
- These are super useful at the beginning of a script to store values for arguments that you might want to change when you run the script on different data.

Examples (run in order):

    $ bio373=/srv/kenlab/USERNAME
    $ workdir=$bio373/work
    $ echo $workdir
    $ ls "$workdir"
    $ echo '$workdir' 
    # Double vs single quotes is subtle, but important. Don't worry too much about it now :)
    $ test="AAA"
    $ test_var="BBB"
    $ echo "$test_var"
    $ echo "${test}_var"

### Control the flow of your commands

Conditionals and loops


