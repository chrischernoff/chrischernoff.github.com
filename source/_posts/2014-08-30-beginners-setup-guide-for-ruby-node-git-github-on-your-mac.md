---
layout: post
title: "Beginner's Setup Guide for Ruby, Node.js, Git, Github, and other things on Mac OS X 10.9"
date: 2014-08-30 11:07
banner: /uploads/2014/08/setup_guide.jpg
---
Last year I wrote a [post](http://burnedpixel.com/blog/setting-up-git-and-github-on-your-mac/) that went through the process of setting up a Mac with a fresh version of Git and authenticating with Github. I formatted it in a way that made it easier for folks who were less familiar with the ins and outs of the terminal (and all of the snags you inevitably hit along the way) to follow along and get up and running in a pretty short amount of time.

Much to my surprise, that guide has seen pretty steady traffic since I wrote it. Every so often folks will ping me saying they were able to hit the ground running without issue thanks to my guide, and I've found that really validating and rewarding.

So I wanted to write an updated version of the guide that not only is bulletproof for OS X 10.9 Mavericks, but throws in how to setup a few common web development tools such as Ruby, rbenv, Node.js, npm, and Grunt. Once Yosemite is out, I'll update again to make sure everything is solid for that as well.

<!--More-->

## Who am I?

I'm a designer/dev based in Los Angeles, but I also really enjoy helping folks become more productive and efficient. This often includes tipping people off to handy apps and utilities, but also branches out to things like helping people save money.

You can read a more in depth version of why I wrote my first guide [here](http://burnedpixel.com/blog/setting-up-git-and-github-on-your-mac/#background), but the gist of it is that setting up your computer for web development can be a pain. Rather than wasting 4+ hours Googling things, reading posts, and getting frustrated when things don't go according to plan, I wanted folks to be able to follow one guide and be up and running quickly. So this is that guide.

There are automated setup tools such as [Boxen](https://boxen.github.com/) that I'd like to investigate for a future guide, but for now we'll keep it "simple."

## Getting started

This tutorial assumes you're using a Mac running at least OS X 10.9. If you are unsure of what OS you have, go up to the top left of your screen, click the Apple menu, and select "About This Mac."

You'll also need to ensure that your user account on your computer has admin privileges and that you know your account's password.

#### A note about the Terminal

  The [Terminal](http://en.wikipedia.org/wiki/Terminal_(OS_X)) application comes pre-installed with OS X, and can be found in the Applications -> Utilities folder.

  The terminal has a variety of uses, and in the cases of Ruby and Node can actually spin up web servers for you while you're developing locally on your computer. For the purposes of this tutorial we'll be using a syntax/command set called [Bash](http://en.wikipedia.org/wiki/Bash_(Unix_shell)) to get things. Terminal is already configured to use this syntax.
  
  When you enter a command and press return/enter, often times the terminal will execute it and complete the task immediately. 

  Sometimes it will log information in the window while it's working, but other times you might feel like it isn't doing anything at all. 

  Some of the commands later in this tutorial can take a few seconds (or minutes) to complete, so don't type anything into the terminal window or close the terminal window until you see it present you with a fresh prompt ending in `yourusername$`.

  For the purposes of this tutorial, commands that I intend for you to type will be preceded with `$`, but don't include that symbol when you enter the commands. It's purely meant as an indicator and reference to the `$` that appears in your terminal prompt.

  Lines that contain comments/notes from me to will be preceded with `#` and will be dimmed. Don't type these either.

  Make sure to press return after typing a command before you enter the next one.


## Install Xcode & the OS X Command Line Tools

[Xcode](https://developer.apple.com/xcode/) is a developer suite Apple offers for free from the Mac App Store. It comes with all the tools you need to build Mac and iOS apps, but also includes basic compilers and libraries needed to install web development tools. You can elect to try installing just the Command Line Tools and forgoing the installation of Xcode itself, but this guide assumes you're installing both.

Let's get started by going to the Mac App Store and installing Xcode (as of writing the current version is 5.1.1). The download is currently a little over 2GB, so it may take a few minutes to complete.

Once it's downloaded go ahead and open up Terminal.app (in /Applications/Utitlies or search for it in spotlight). Let's install the Command Line Tools

```bash
# Type this and press the return key. 
# Note that there are two dashes, not one.
$ xcode-select --install
```

A prompt will appear asking you if you want to install the Command Line Tools, click Install. This may take a couple minutes.

Once the tools are successfully installed, we'll need to accept the Xcode license. This is a bit silly, as we're doing it by checking the version of gcc so bear with me.

Executing this will ask you for your account password, you won't see any characters as you type. Enter your password and press return. If you make a mistake, just press return and it will let you attempt to enter your password again.

```bash
$ sudo gcc --version
```

Press return to view the agreement. Once opened, the agreement is long, so press space until you reach the bottom and are prompted with:

>By typing 'agree' you are agreeing to the terms of the software license agreements. Type 'print' to print them or anything else to cancel, [agree, print, cancel]

Type agree and press return. You'll be returned to your terminal session and should see something like:

>Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 5.1 (clang-503.0.40) (based on LLVM 3.4svn)
Target: x86_64-apple-darwin13.0.0
Thread model: posix

Now we're getting somewhere!

## <a id="install-homebrew"></a>Install Homebrew

Homebrew is an awesome tool that makes installing packages, libraries, utitlies, etc a breeze. It is what we will use to install subsequent items in this guide such as Ruby, Node, and Git. Trust me, it is way easier and more convenient to install (and upgrade in the futrue) all of the aforementioned tools with Homebrew than by hand.

Run the following command in the terminal:

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Press return
```

Once complete, you should see:

>==> Installation successful!
==> Next steps
Run `brew doctor` before you install anything
Run `brew help` to get started

So let's follow its advice and run:

```bash
$ brew doctor
```
If all went well during installation, brew doctor should indicate good things:

>Your system is ready to brew.

If so, awesome. If not, there is a wide range of errors depending on if you've tried installing Xcode, the Command Line Tools, and/or Homebrew before. Feel free to reach out to me for help, and Moncef Belyamani has a [helpful collection](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/#troubleshoot-homebrew) of common Homebrew issues that might help.

One tip if you've attempted to install Homebrew in the past but got stuck is to completely remove your old installation:

```bash
# Only run if you've attempted to install Homebrew 
# before and got stuck. This will ask you for your 
# user password. Type it and press return
$ sudo rm -rf /usr/local/Cellar /usr/local/.git && brew cleanup
$ cd /usr/local
$ sudo mv -v Library Library.old
$ cd ~
```

Now try going through the [Homebrew install](#install-homebrew) steps again.

## Updating your path environment variable

Another thing we should change before we go installing more tools is your unix `$PATH` environment variable. This is one of the common sources of issues when new users try to install command line tools/packages (I know it was/is for me). Think of the path as the series of folders in which your computer will look for a particular program or package. Consider the following example:

1. You have the script jumpingjacks 1.0 installed in folder A.
2. You install jumpingjacks 2.0, but when you install scripts they end up in folder B.
3. Your `$PATH` is only set to look in folder A.
4. When you run "jumpingjacks", your computer will open jumpingjacks 1.0.
5. You update your `$PATH` to look in folder A, then folder B.
6. When you run "jumpingjacks", your computer will *still* open jumpingjacks 1.0, because that is the first version that it found when it looked through the folders you asked it to.
7. You update your `$PATH` to look in folder B first, _then_ folder A.
8. Now when you run "jumpingjacks", your computer will open jumpingjacks 2.0.

Hope this silly example makes things a little more clear. Where it applies to us is we want OS X to look in the folder where Homebrew will be installing many of the packages we will ask it to. This folder is `/usr/local/bin/`. By default, your computer will look in `/usr/bin` first instead. You can check your `$PATH` by doing the following:

```bash
$ echo $PATH
```
It will most likely come back with something like this:

>/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin

`/usr/local/bin` is all the way at the end! To change this, we'll update a configuration file called your `bash_profile`. This file will contain any customizations you want to make to your Bash environment. Run the following to update your `bash_profile`:

```bash
$ echo export PATH='/usr/local/bin:$PATH' >> ~/.bash_profile
$ source ~/.bash_profile
$ echo $PATH
```
After running the last command, you should see the following:

>/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin

Notice how `/usr/local/bin` is at the beginning now? This means as we install things with Homebrew, your computer will see those versions first.

## Install Git
{% img right /uploads/2013/03/git.jpg %}

_"[Git](http://en.wikipedia.org/wiki/Git_(software)) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency."_

We need to install Git onto your computer. It won't have an icon in your dock, but it can be used by the Terminal.

The OS X Command Line Tools come with a version of Git, but we want Homebrew to manage our installation of Git so we can use a newer version and have an easier time staying up to date in the future. Let's install the latest version of Git using Homebrew:

```bash
$ brew install git
```
After it's done installing, let's double check that your computer is referencing the latest version we just installed:

```bash
$ which git
```
You should see `/usr/local/bin/git` returned to you. If you see `/usr/bin/git` then you may have issues with your `$PATH`. Again, please feel free to reach out, but a starting point may be to empty out any `$PATH` related statements in your `.bash_profile` _or_ perhaps your `.bashrc` file.

Assuming you see the correct git path, let's move on!

## <a id="setupgithub"></a>Get a Github account
{% img right /uploads/2013/03/github.jpg 300 %} 

"_[GitHub](http://en.wikipedia.org/wiki/GitHub) is a web-based hosting service for software development projects that use the Git revision control system._"

Go to [Github.com](http://www.github.com) and create a free account if you haven't already. For the purposes of this guide a Free account is just fine.

## <a id="gitidentification"></a>Configuring Git identification

Once you have your Github account ready, let's setup your local Git installation to correctly identify you and authenticate with Github.

```bash
# Set your username
$ git config --global user.name "Your Name Here"

# Set your email address
$ git config --global user.email "your_name@domain.com"
```
Make sure the email address you enter is shown in the list in your [Github Email Settings](https://github.com/settings/emails), as this is how Github maps commits to Github users. If your desired email isn't in that list, you can add it.

Since we installed Git with Homebrew, a handy helper has been included along with Git that will remember your Github username and password so you don't have to log in every time you connect to Github from the Terminal.

To tell Git to use this helper, run this:

```bash
$ git config --global credential.helper osxkeychain
```

You will now able to access Github repositories using the HTTPS method. There's a very good chance that this is the only method you will need to access repositories, and you will only have to enter your credentials the first time you connect to Github.

#### <a id="sshkeys"></a>SSH Keys (optional step)
_"[SSH](http://en.wikipedia.org/wiki/Secure_Shell#Definition) uses public-key cryptography to authenticate the remote computer and allow it to authenticate the user, if necessary. There are several ways to use SSH; one is to use automatically generated public-private key pairs to simply encrypt a network connection, and then use password authentication to log on."_

If you have a specific reason that you need to access Github using SSH instead of the default HTTPS method, you can configure your computer to do so below. Otherwise, proceed to [Installing Ruby](#installing-ruby).

An SSH key is an algorithmically created phrase that only your computer and the device you're connecting to should know. If Github is aware of the key your computer is using, you won't have to enter your Github username/password every time you connect using SSH. Note, if you use HTTPS to clone your repository, SSH will not come into play.

##### Check for pre-existing SSH keys on your computer
Let's see if your computer has one or more keys already created. Another app or script you used may have done this.

```bash
# The below command will copy your key 
# to your computer's clipboard.
$ pbcopy < ~/.ssh/id_rsa.pub
```
If you get the response "No such file or directory", skip to [Generate a new SSH Key](#generatenewkey).

Otherwise, move on to the next step so we can let Github know to look for your existing key.

##### <a id="add-your-ssh-key-to-github"></a>Add your SSH key to Github
Now we need to let Github know the contents of your SSH key.

1. Visit your [account settings](https://github.com/settings/ssh).
2. Click **Add SSH key**.
3. Enter a descriptive title for the computer you're currently on, e.g. "Work iMac" into the Title field.
4. Paste your key into the **Key** field (it has already been copied to your clipboard).
5. Click **Add Key**.
6. Enter your Github password.

Now let's test that it all worked.

```
 # Attempt to connect to Github using your SSH key.
 # Don't change the address shown below
 $ ssh -T git@github.com
 
 # You may see the following warning:
 The authenticity of host 'github.com (207.97.227.239)' 
 cant be established.
 RSA key fingerprint is ...
 Are you sure you want to continue connecting (yes/no)?
 
 # Type yes and press return
 # You may have to enter your recently
 # selected passphrase.

 # You should then see:
 Hi username! You've successfully authenticated, 
 but GitHub does not provide shell access.
```
You're good to go with Git & Github, skip down to [Installing Ruby](#installing-ruby) to continue.


##### <a id="generatenewkey"></a> Generate a new SSH key (if you didn't already have one)
We're going to create a new SSH key to use with Github.

```bash
# Ensure that you are in your ~/.ssh folder
$ cd ~/.ssh

# Create a new ssh key using the 
# email address you used to log into Github.
$ ssh-keygen -t rsa -C "your_email@domain.com"
```

When it asks you to enter a file name in which to save the key, just press return/enter (leave the prompt blank).

You will then be asked to enter a passphrase and confirm it. **Don't** make this blank, but do make sure it's a password you would remember if you had to recall it in the future.

Press return after each time you've entered your selected passphrase. You won't see the characters or bullets, the cursor will stay in the same spot as if you aren't typing. 

If you make an error entering your password one of the times, just press return and it will prompt you to try again.

Once you've successfully set your passphrase, the terminal will report that your key has been saved and will present you with some sweet ASCII art. Copy your key to your clipboard using the following command:

```bash
# The below command will copy 
# your key to your computer's clipboard.
$ pbcopy < ~/.ssh/id_rsa.pub
```

Now that we've created your key, return to [Add your key to Github](#add-your-ssh-key-to-github).

## <a id="installing-ruby"></a>Installing Ruby with rbenv

A lot of nifty web apps and sites are created using Ruby (like this very blog!). While I've struggled in the past to figure out a sane way to keep Ruby running smoothly, rbenv makes things pretty straight forward. Your mac will already have a version of Ruby installed, but it is always ideal to be able to control exactly which version of Ruby you have installed along with the gems necessary for each of your projects.

First let's install rbenv and ruby-build:

```bash
$ brew install rbenv ruby-build
```

Once complete we'll add this configuration to your `.bash_profile`:

```bash
$ echo eval "$(rbenv init -)" >> ~/.bash_profile
$ source ~/.bash_profile
```
Test that rbenv is working by simply typing:

```bash
$ rbenv
```

If rbenv is correctly installed, you'll see a list of commands that looks something like:

>rbenv 0.4.0
Usage: rbenv `<command>` `[<args>]`
Some useful rbenv commands are:...

Now it's time to install a version of Ruby. Let's see what versions are available:

```bash
$ rbenv install -l
```

This list can be quite long, but if you scroll up a bit you should see a number of items that are just numbers (for example `2.1.2` or `2.0.0-p481`). Let's install the latest stable version of Ruby, which as of writing is `2.1.2`. You can double-check what the latest official stable version is at the [Ruby Lang](https://www.ruby-lang.org/en/downloads/) site.

If there are newer stable versions, just replace `2.1.2`. with whatever that number is in the steps below:

```bash
$ rbenv install 2.1.2
```
Once this completes, you can run `ruby --version` to see what version of Ruby your system has installed...

You'll probably notice that the version number returned isn't the one we just installed (`2.1.2`)! That's because we need to tell rbenv to set `2.1.2` as the default version for your computer:

```bash
$ rbenv global 2.1.2
```

Now run `ruby --version` one more time and you should see 2.1.2 returned as the version number:

>ruby 2.1.2p95 (2014-05-08 revision 45877) [x86_64-darwin13.0]

Awesomeness! Let's move on to Node.

## Install Node.js and npm

Besides being the new hotness for a wide range of web projects, Node and it's handy package manager npm power quite a few other tools (like Grunt) that make web development and designing easier. Let's go!

```bash
$ brew install node
# npm is installed when node is installed, neat!
```

Assuming all goes well, you are now able to install and run node based projects. To verify everything is as it should be, run:

```bash
$ npm version
```
This should return something like the following:

>{ http_parser: '1.0',
  node: '0.10.31',
  v8: '3.14.5.9',
  ares: '1.9.0-DEV',
  uv: '0.10.28',
  zlib: '1.2.3',
  modules: '11',
  openssl: '1.0.1i',
  npm: '2.0.0-beta.2' }

If so, you're good to go with node and npm!

## Install Grunt

Let's test out the power of npm by installing Grunt. You'll notice along the way that as Grunt is installed numerous packages (called "dependencies") are installed as well. This is the flexibility of the [Unix Philosophy](http://en.wikipedia.org/wiki/Unix_philosophy) that prefers building an application or tool upon numerous small and interchangeable parts. As one piece becomes updated or outdated, it can be easily switched in or out.

```bash
$ npm install -g grunt-cli
```

Afterwords, run `grunt` to verify it's installed. If it is, it will complain that there is no Gruntfile, and that's because we're not working on a project.

Grunt is ready.  

## Conclusion

That about covers the basics of installing and configuring Git, Github, Ruby, rbenv, Node.js, npm, and Grunt on your Mac. You should now be able to accomplish a wide variety of tasks and are in a great position to keep your system updated and easily customizable. Often times, projects you may find online only require a `brew install`, `git clone` and/or an `npm install` to get started! Never before has web development been so powerful, so open, and so easy to participate in.

In future posts I hope to detail my use of such projects, along with the some of the customizations I've made my system to make me more productive during the day.

Thanks for reading!

## Feedback

If there are any steps/instructions I've written that have been outdated by newer information/technology, are simply wrong, could be explained better, or you're just stoked to have made it through this process, please feel free to contact me on Twitter where I'm [@burnedpixel](https://www.twitter.com/burnedpixel).