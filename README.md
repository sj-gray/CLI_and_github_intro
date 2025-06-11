# Introduction to CLI and GitHub 
Introduction to CLI and GitHub for GRAD-MAP summer scholars 2025. 

## Background 
The terminal (aka command line interface) is where you can give instructions to tell your computer what to do. Before windows/menus/cursors/mice, it was all a user had to utilize a computer. Modern computers still work the same way behind the scenes, but also have the graphical user interface we’re all used to. Terminal applications provide a place where you can access a more direct way of interacting with the computer. 


## Getting started
Open a new shell (Terminal on Macs, the Ubuntu24 we installed yesterday through WSL on Windows). When the shell is first opened you are presented with a **prompt**, indicating that the shell is waiting for input. The shell typically uses `$` as the prompt, but it may use a different symbol. The prompt is followed by a **text cursor**, a character that indicates where your typing will appear. The text in front of the prompt indicates the username, the computer you're running on, and the directory you're in. 

Let's get our bearings and try our first command. This command will print the current working directory you're in:
```
pwd
```
This will give you the full path of the directory you're in (which is sometimes shortened in the area before the prompt). If you're on Mac, this should look something like `/Users/YOUR-USERNAME`. If you're on WSL, it will look something like `/home/YOUR-USERNAME`.

Now that we have an idea of where we are, let's see if there is anything else in the directory we are in. To do this, we can use `ls` which is short for listing. This command list the contents of the current directory: 
```
ls
```
If you're on Mac, this command will list things like `Applications`, `Desktop`, `Documents`, `Downloads`, etc. In WSL, it's likely that you're home directory is currently empty, so this will show nothing and immediately jump to another prompt line. If you want to change your current directory to be able to access your `Desktop`, `Documents`, and `Downloads`, you can use the `cd` (change directory) command followed by the directory you want to change to. I *think* it will look something like 
```
cd /mnt/c/Users/$YOUR-WINDOWS-USERNAME
```

The part of the operating system responsible for managing files and directories is called the **file system**. It organizes data into files and directories (aka folders), which can hold other files or other directories. 

Several commands are frequently used to create, inspect, rename, and delete files and directories. Directories are like *places* -- at any time while we are using the shell, we are in exactly one place (our current working directory). Commands typically read and write files in the current working directory. 

`ls` has lots of other options. Some options that are commonly used include:
* `-l` Selects the **l**ong output format (includes the file type, permissions, owning user/group, size, and last-modified timestamp)
* `-h` Output sizes as so-called **h**uman readable by using units of KB, MB, GB intead of bytes.
* `-t` Sort the list by modification **t**ime (default sort is alphabetically)
* `-c` Sort the list by last attribute (status) change time

## GitHub setup 
If you don't have one already, take a minute to make a github account. 

### SSH Keys
Once you have that set up, we will add a new SSH Key to your github account. SSH stands for Secure Shell; essentially it is a protocal that allows for secure remote login and file transfer. 

First, let's check for existing ssh keys (if you haven't heard of ssh keys yet you likely don't have any, but checking makes for good command line practice). In your terminal, enter 
```
ls -al ~/.ssh
```
This will list all the files in your `.ssh` directory, if they exist. If not, there won't be any output. If there is output, check to see if one of the files looks like **id_rsa.pub, id_ecdsa.pub,** or **id_ed25519.pub**. If such a file exists, wait to follow the instructions (or skip down to **Adding your SSH key to the ssh-agent** below).

#### Generating a new SSH Key
To generate a new ssh key, copy and paste the text below into your terminal, replaceing the email with the email associated with your GitHub account. 
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
When you're prompted to "Enter a file in which to save the key", you can press **Enter** or **Return** to accept the default file location. 
```
> Generating public/private ALGORITHM key pair.
> Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
```
At the prompt, type a secure passphrase. For more information, see [Working with SSH key passphrases](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases). 

#### Adding your SSH key to the ssh-agent
1. Start the ss-agent in the background by running:
   ```
   eval "$(ssh-agent -s)"
   ```
2. Add your SSH private key to the ssh-agent
   ```
   ssh-add ~/.ssh/id_ed25519
   ```
3. Add the SSH public key to your account on GitHub. For these steps, see [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).


### Cloning a GitHub Repository and More CLI
Once you've followed the steps to add your SSH key to your GitHub account, you'll be able to start pulling repositories from GitHub via SSH. Let's go through an example of that with this repository now. 

In your terminal, make a new directory called gradmap using the `mkdir` (make directory) command:
```
mkdir gradmap
```
Navigate into this new directory using the `cd` (change directory) command: 
```
cd gradmap
```

Scroll up to the top of this page & look for the the green `<>code` button. Click on it, navigate to the `SSH` tab, and copy the `git@github.com:sj-gray/CLI_and_github_intro.git` (or copy it from here, but following the steps described will be good practice). Now go to your terminal, type in `git clone ` and then paste the `git@github.com:sj-gray/CLI_and_github_intro.git` you should have copied to your clipboard. If you're using WSL/Linux, you will have to use `ctrl+shift+v` or right-click then select Paste. Macs can use the typical `cmd+v`. 

Change into the directory of the repo you just pulled using 
```
cd CLI_and_github_intro
```

Create your own branch (and switch to it) using 
```
git checkout -b <branch-name>
```
Where you can replace `<branch-name>` with the desired name of your branch. In this example, you can just use your name. The `git checkout` command lets you navigate between branches, and the `-b` option specifies that you want to create a new branch then switch to it. 

#### Create a new file 
There are various text editors that can be used within the terminal; my go-to is `vim`, but you can test out others if you're curious (`nano` and `emacs` are examples of other terminal text editors). To start, you can create and open a new file with 
```
vim notes.txt
```
Once you run this, your terminal will convert into a mostly blank screen with your text cursor at the top, and a message at the bottom specifying what file you just opened and the fact that it is a new file `"notes.txt" [New]`. `vim` has a few specific commands itself; the most common are listed below:
* To start typing, you first have to go into insert mode by pressing `i`
* When you're finished editing, press the "Esc" key to exit insert mode
* Outside of insert mode, you can save your changes and quit vim using the command `:wq` (write and quit), or save without writing using `q!`.

So, to add some text to your new `notes.txt` file:
1. Press `i` (after you have the file open with vim)
2. Type a note, such as `Woooo I'm learning CLI and git and more!`
3. Press `esc`
4. Type `:wq` to save your changes and quit vim.

#### Check git status, add, commit, and push changes
Now that you've made a new file in this github repo, you can keep track of what changes you've made and what branch you're on using 
```
git status
```
After running this command (and if you've been following along above), you should see something like 
```
On branch sjgray
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	notes.txt

nothing added to commit but untracked files present (use "git add" to track)
```
To store your local changes on github, first add the file(s) you've changed:
```
git add notes.txt
```
Then `commit` your changes with:
```
git commit -m "Wrote some notes"
```
The option `-m` allows you to write a commit message, which serves as a short description of what changes you've made that you would like to add to the repo. 
Finally, to put your changes on github, run
```
git push --set-upstream origin <branch-name>
```
where you replace `<branch-name>` with the name you gave your branch above. If you're unsure of what branch you're on, you can check with `git status` or `git branch`. 
