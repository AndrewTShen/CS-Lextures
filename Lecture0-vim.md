# Lesson 0: Vim
**Written by Andrew Shen**  
**Last Updated: 09/07/2019**

### Glossary
1. [Introduction](#introduction)
2. [Vim Modes](#vim-modes)
3. [.vimrc](#.vimrc)
4. [Conclusion](#conclusion)

### Introduction
##### Why do we need text editors?
At the base of any programming, we need to be able to write code quickly and efficiently. As a result, we need to choose a suitable and reliable test editor that will not only allow us to traverse large amounts of code quickly, but also be flexible and easy to use.

There are many suitable text editors out there and many of which have similar functionality and capabilities, it's up to you to find a good text editor that will suit your needs.

##### So what is vim and why is it good?
Vim is a very popular text editor with a lot of support online and user-built plugins that can make programming 100x more efficient, we will get to adding plugins later! It is very easy to set up and it is included on nearly all mainstream operating systems that are in common use so it is great when you `ssh` to another computer or server. Although at first it might take a little while to get use to, once you get the hang of it, it'll be a lot of fun to use.

---

### Vim Modes
Let's get started by opening terminal (or command prompt, but I'll be using terminal for the remainder of this lesson) and typing in `vim test.txt`. This will go ahead and open up a new text file in our currently directory. You might notice that if you try typing something, you cannot directly edit the file, this is because on boot up, you are currently in something called *normal* mode. In vim, tthere are 3 different modes most commonly used, these are: *normal* mode, *insert* mode, and *visual* mode. Here's a brief summary of each of the modes:

- In normal mode, we can call vim commands to perform quickly perform several tasks.
- In insert mode, we can edit the text.
- In visual mode, we can select large portions of our text to perform an one large operation.
##### Insert Mode
Since we would like to be able to actually edit our text file rather than just look at it, let's enter into insert mode. We do this with <kbd>i</kbd>. Let's start by typing 

`Hello World!`  
`It's good to be typing in vim!``

 Now, if we press the <kbd>esc</kbd> key, we can reenter normal mode. Here are ways some of the more common ways we will enter insert mode:

- <kbd>i</kbd> enter insert mode at cursor.
- <kbd>a</kbd> enter insert mode after cursor. 
- <kbd>I</kbd> enter insert mode at the beginning of the line.
- <kbd>A</kbd> enter insert mode at the end of a line.

Note: everytime we want to re-enter normal mode we need to press the <kbd>esc</kbd> key.

##### Normal Mode
Notice now, that you can use the arrow keys to move around the code as you normally would. One thing you might not be used to however is that you can use <kbd>j</kbd> to move up, <kbd>k</kbd> to move down, <kbd>l</kbd> to move left, and </kbd> to move right. Try it out an move around! These key are a more convientent way of moving around. Here are a couple more common keys you'll be using in normal mode:

- <kbd>w</kbd> move forward a **w**ord.
- <kbd>b</kbd> move **b**ackward a word.
- <kbd>$</kbd> move to the end of the line.
- <kbd>0</kbd> move to the start of the line.
- <kbd>d</kbd> delete, this deletes to where you move next, or a selected section of text. <kbd>d</kbd> is incredibly powerful andworks pretty much like delete.
- <kbd>u</kbd> undo an edit.
- <kbd>p</kbd> paste the copied buffer.
- <kbd>ctrl</kbd> + <kbd>r</kbd> redo an edit.

Note you can type a number before an command to do it any number of times.

##### Visual Mode
If we press <kbd>v</kbd> we can enter visual mode. This mode is essentially highlight. You don't really use it much but you can delete large sections of code. <kbd>V</kbd> (captial V) will let you select an enter line at once. <kbd>V</kbd> +<kbd>ctrl</kbd> lets you select entire blocks of code at once. This is good for actions like copy pasting and deleting large chunks of text. Again, to exit visual mode we use <kbd>esc</kbd>.

##### Deleting and Pasting
Some quick notes about using delete. If you use delete with:

- <kbd>;</kbd> delete the previous character.
- <kbd>l</kbd> delete the next character.
- <kbd>j</kbd> delete the current line and next line.
- <kbd>k</kbd> delete the current line and previous line.
- <kbd>$</kbd> delete from the cursor to the end of the line.
- <kbd>0</kbd> delete from the cursor to the start of the line.

Everytime you delete something, it is saved to the copy buffer. Essentially, each time you delete something it acts as a cut. So, you can use <kbd>p</kbd> to paste the last thing you deleted. Setting up just regular copy can be a bit of a hassle, but on most vim it is usually <kbd>\"</kbd>, <kbd>\*</kbd>, <kbd>y</kbd> in that specific order.

##### Saving and Writing
So, after you have written whatever you want to write in that file, we can save it in normal mode. We type <kbd>:</kbd>, which allows us to tell vim to perform a certain action. We can then type <kbd>x</kbd>, <kbd>enter</kbd> to save and quit out of our project. Some other common usages include:

- <kbd>w</kbd> + <kbd>enter</kbd> to write our changes, but **not** close out of the file/buffer.
- <kbd>q</kbd> + <kbd>enter</kbd> to quit out of vim, but if we have made changes, we will need to save them.
- <kbd>q!</kbd> + <kbd>enter</kbd> to quit out of vim and discard all changes.

Now that we have the basics down, we can move on to doing the cool stuff.

---

### .vimrc
##### What are dotfiles and why do we use them?
Dotfiles are files that are proceed with a `.` and usually hold information about the system preferences. In our case, `.vimrc` can hold infromation about how we want vim to work. The contents of this file will run everytime vim is started. Here we can make changes to how vim functions and looks by changing the default settings as well as installing plugins and tools to make editing more conviennent.

##### How do we make a .vimrc file?
We can make a `.vimrc` file by simply going to our home directory with `cd ~` and then using `vim .vimrc` to open and edit the file. Here's a couple lines of vim to get you started.

- `set tabstop=8 softtabstop=0 expandtab shiftwidth=4 smarttab`, this gives you some spacing and tab formatting.
- `set mouse+=a`, this gives you the ability to use your mouse to click into different spots.
- `syntax on`, this lets you have syntax highlighting.
- `set nu`, this displays line numbers.
- `set clipboard=unnamed`, this allows you to copy to clipboard.
- `set backspace=indent,eol,start`, this allows you to backspace over an end of line.
- `set autoindent`, this allows vim to autoindent for you.

In this folder, you can find my own `.vimrc` file that I use, but I would heavily recommend against just copying things that are in there and instead adding each line at a time so that you know what your vim is doing. One of the main advantages of vim is that without plugins, it is relatively barebones and you can build it up as you need it and prefer it. *It is easily customizable.*

##### Okay this is all cool, but how to I make vim the powerhouse that it really ought to be?
Probably my favorite part of vim is the plugins. The plugins make vim an absolute unit. You can traverse files, auto complete code, and almost anything else you can think of. To install plugins, I like to use junegunn's [vim-plug](https://github.com/junegunn/vim-plug) for numerous reasons, but any plugin manager works fine for any plugins (regardless of what its github page recommends). You can install `vim-plug` by pasting  

`curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim`  

into your terminal and adding  

`call plug#begin()`  
`call plug#end()`  

into the `.vimrc` file. Then, in between any of those lines we can add in our plugins in the format.  

`Plug [github account]/[repository name]`. After that, we can reload vim, by doing `: so ~./vimrc` as a command in normal mode, or by exiting and reopening vim. After, we can use `PlugInstall` to install the plugin that we just added and we are set!

---

### Conclusion

Vim is a absolute powerhouse and with the right settings and tools, we can pretty much do anything we want (with a bit of practice of course). You can code, write markdown files (as I am currently doing now), among many other usages. There are so many other cool tricks and shortcuts that I couldn't get to in this lecture, but there are many other guides and resources that can show you faster and faster ways to do things. If you have any questions, regardless of who is reading this, feel free to write an email to `shenandrew95@gmail.com` and I'll do my best to resopnd to you.`
