# Terminal

On Macs, you open the Terminal. This is your window into the computer. By default, bash is running. Most of the commands for Linux bash and Mac bash are the same or similar enough that you can switch between the two fairly easily.

The `$` symbol is the beginning of the prompt. This is where you type commands and in most documentation, a command line command is indicated with a `$` at the beginning and is NOT meant to by typed. 

# First time login

You need your BFabric username and password to login. After your account has been registered, you now have a `/home/USERNAME` directory already there. This directory is always backed up by the FGCZ. But there is limited space and is not meant for long storage of files or data. Whenever you login with `ssh`, it is automatically setup so that you are in that directory. This is considered the `home` directory and is synonymous with the `~` symbol in path names.

To login, from the terminal type:

    $ ssh USERNAME@fgcz-c-047

It will ask you to enter you password. Use the BFabric password. You will not be able to see what you typed or even how many characters you typed, so in order to get around that, check below!

# Setting up "passwordless" secure login

On your local computer

    $ cd ~/.ssh
    $ ssh-keygen -t rsa -b 4096 #Press [Enter] for each line unless you want a passphrase, but then you are entering the passphrase each time you try to log in
    $ ssh-copy-id gwynhn@fgcz-c-047 #This copies the public key to the fgcz server in ~/.ssh/authorized_keys
    $ vi config # Or any text editor. You just need the block below; indentation is necessary!

    Host f47
        HostName fgcz-c-047
        User gwynhn
        IdentityFile ~/.ssh/id_rsa

Should be able to login using `ssh f47` without password...might ask for it once, but not after that first time.

