# bwa_install_M1_MacOS_2022
Kenneth P. Mitton, Ph.D. FARVO
Eye Research Institute
Oakland University
Pediatric Retinal Research Laboratory
Rochester, Michigan
mitton@oakland.edu

How to install bwa for DNA sequencing on a M1 Mac. The regular make routine must be modified for the newer non-Intel Macs.
I found some guidance and reference to them here and also provide the commands that I found worked well to get bwa installed on an M1 Mac running Big Sur. 
The install information for bwa works fine on an Intel Mac, as you can do in a typical bash terminal window with:
## Routine installation on Intel Macs
```
git clone https://github.com/lh3/bwa.git

cd bwa

make
```
## Problem using the routine make process on M1 Macs
On your M1 Mac all is fine as far as geting bwa.git to transfer files, but sadly the `make` process reports errors. The solutions I found indicated I
had to employ `wget1` to swap a different library during the compilation step called by by `make`.

It can get more annoying if you do not have `wget` installed, so the best solution I found was to use Home Brew to install `wget`, then you are on 
your way to getting `bwa` installed. 

## Step 1: Install `Homebrew` and then `wget`
The terminal commands required are below. Note that this can also be used as script that I have also loaded into this repository if you want to fix up several Macs. 
The script version will start installation of both Home-brew and then wget. If you just need to install one of these, simply remove the install 
section you do not need from this script, rename and save it to run in bash. Note that installations may prompt you to confirm continue, and the home brew installation can take 10-30 minutes depending on how fast the components download to your computer. You can also just run the commands below in order manually. 

### Install Homebrew
Once you you have Homebrew, it is a tool that will be used to install many more packages. If you already have Homebrew installed you can skip to the next section
>"Install wget using home brew"

If not, install Homebew

Run this command: 

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Note after it installs the output will also provide you two commands to run to ensure your path for `brew` is set and available.
Make sure to copy them and run them. You do not need to add a path to your .bash_profile, as the first command you will be asked to run will add this
line to the file .zprofile in your home directory:  `eval "$(/opt/homebrew/bin/brew shellenv)"`. Then the second command you will be asked to run
is that same command line again added to .zprofile: 

`eval "$(/opt/homebrew/bin/brew shellenv)"` 

This will modify PATH immediately without needing to log out and in again. In the future you will not need to run the last command again.

Once you complete that process, confirm that you have `brew` now in your PATH. Use command:

`Echo $PATH`

or command:

`which brew`

to return the full path to brew. It should report back `/opt/homebrew/bin/brew`

### Install wget using home brew
If you already have brew available, or just installed it to make it so, you can now install wget with:

`brew install wget`

Once this completes, you can confirm that wget is available with `which wget`, and should see the returned directory:

 `/opt/homebrew/wget`
 
Now you are ready for step-2 to install bwa, finally!

## Step-2 Install bwa on your M1 Mac.

You can still use git to get the bwa files as you would on an Intel Mac:

`git clone https://github.com/lh3/bwa.git`

Then you can change into the bwa directory:

`cd bwa/`

Then try this to swap in a different library, make BWA using *make clean all* which will first remove any binaries and object files 
in your bwa folder (which you may have from the failed compile attempts that led you to find my guide here), then do a fresh compile.

First command, sed, is required to replace a string in a file ksw.c with *sse2neon.h*.:

`sed -i -e 's/<emmintrin.h>/"sse2neon.h"/' ksw.c`

Second command, uses wget to get the file required:

`wget https://gitlab.com/arm-hpc/packages/uploads/ca862a40906a0012de90ef7b3a98e49d/sse2neon.h`

Third and finally, you can now make the bwa compile work. Make sure your present working directory is still bwa of course, run:

`make clean all`

If the process works, when you list the files in bwa folder with `ls -l -h`, there will be one new file just named `bwa` without any suffix in its file name. 
That should be the executable bwa. Bwa is not yet added to your PATH so open your .bash_profile, or make one in your home directory, maybe with vim text editor:

`cd ~`

`vim .bash_profile`

and add the location of bwa to your path. Lines like this added to your profile should work:

`PATH="/Users/ken/Applications/bwa:${PATH}"`

`export PATH`

Close your terminal program, then restart terminal and check if bwa is now available to you:

`which bwa`

`echo $PATH`

and `bwa` alone to get brief bwa options guide and info. Now you have bwa on your M1 Mac platform.

THE END


### SOME REFERENCES I USED TO ASSEMBLE THE ABOVE SOLUTION
Ken Mitton. Mitton@oakland.edu in 2022
1. For installing wget: https://www.cyberciti.biz/faq/howto-install-wget-om-mac-os-x-mountain-lion-mavericks-snow-leopard/
2. For installing bwa for M1 Mac compilation: https://stackoverflow.com/questions/65555170/unable-to-run-make-command-for-bwa-on-apple-m1-on-mac-os-big-sur

