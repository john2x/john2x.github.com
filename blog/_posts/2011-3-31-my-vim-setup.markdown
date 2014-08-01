---
title: My Vim Setup
date: 2011-3-31
layout: post
permalink: /blog/my-vim-setup
category: blog
tags: vim
---

[Vim][] is my preferred editor to write in, code or otherwise. Whenever I am forced to use another program to write, I feel uncomfortable and sometimes accidentally type in stuff like "dw" or ":w" and I find myself pressing `<Esc>` all the time. 

This post will showcase the plugins I use and some of the settings in my [vimrc][] file which make using Vim much easier. 

<h2 id="plugins">Plugins</h2>

These are the plugins I can't live without and which I find myself using all the time. 

### NERDTree ###

If I can only use one plugin in Vim and had to choose which one, I'll pick [NERDTree][] in a heartbeat. NERDTree is a file explorer plugin that gives a "drawer" like feature in Vim. Activate it, and you get a nice file explorer on the left, so you can easily find the files you want. 

You can bookmark directories with the command `:Bookmark bookmark_name`. Now, when in the NERDTree window, pressing `B` brings up all your bookmarks. Pressing `cd` on a folder node changes the current working directory into the highlighted node. 

![NERDTree]({{site.data.global.static.images}}{{page.url}}/NERDTree.png)

### MiniBufExpl ###

Managing buffers is probably the most confusing aspect of Vim. I still don't quite understand how to manage them, to be honest. But that's what [MiniBufExpl][] is for. 
It arranges your buffers in a row on the top of the window like tabs in a browser or other editors. 
Everytime you open a file and start a new buffer, it is listed on top. You can then press `<Ctrl-Tab>` and `<Ctrl-Shift-Tab>` to cycle between them. And since the plugin displays the buffer number beside the its name, you can use the `:bN` command to quickly switch to a buffer number (where `N` is the number).  
The plugin also highlights buffers which have unsaved changes in them, which can be very handy when you're editing a lot of files. 

![MiniBufExpl]({{site.data.global.static.images}}{{page.url}}/MiniBufExpl.png)

### Bclose ###

[Bclose][] is another plugin which helps with buffer management. It offers an easy way to easily close buffers and when the last buffer is closed, it creates a "scratch" buffer in place of the last one. It makes Vim act more like other editors you may be used to. 

Calling `:Bclose` closes the current buffer and opens the next available one or creates a "scratch" or empty buffer if it can't find the next one. I mapped `:Bclose` to `<leader>x` so I can easily close buffers with a simple keystroke. 

### Surround and Repeat ###

[Surround][] offers shortcuts to easily surround words with parentheses, quotes and even HTML tags. (e.g. Typing `ysiw"` surrounds the current word with quotes and `ysiw<tag>` surrounds the word with `<tag></tag>` tags. `ds<tag>` deletes the nearest surrounding `<tag></tag>`.)

[Repeat][] extends the default `.` or "repeat" command in Vim so it can be used with Surround, among other things. 

### EasyMotion ###

As if Vim's movement keys weren't good enough, [EasyMotion][] is a very welcome addition to my setup. It provides quicker movement by allowing you to "pinpoint" the location you want to place your cursor. The [animation][easymotionanimation] on the project page probably does a better job at showing what I mean. 

![EasyMotion]({{site.data.global.static.images}}{{page.url}}/EasyMotion.png)

### Pathogen ###

[Pathogen][] provides an easy way to manage your plugins by allowing you to place each plugin in their own folder. Pathogen then appends each plugin's folder to Vim's path so they can be loaded and used. 
It has also given me the opportunity to clean up my `~/.vim` folder and to upload it to [Bitbucket][hgdotfiles] and [Github][gitdotfiles].

![Pathogen]({{site.data.global.static.images}}{{page.url}}/Pathogen.png)

### Other Plugins ###

These are plugins I have installed but I rarely use, if at all. 

- [Fugitive][] - A plugin for using Git within Vim. I'm switching over to Mercurial but I still use Git from time to time. 
- [SnipMate][] - Allows you to create and use snippets to quickly generate pieces of code. I don't know why I don't use this much, probably because I could never remember its key bindings. 
- [LustyJuggler][] - Another buffer management plugin. Allows for faster switching between open buffers. I find myself using `:bN` most of the time though. 
- [Taglist][] - A source code browser. I mapped it to `<F4>` to get a quick overview of a piece of code. 


<h2 id="vimrc">My vimrc</h2>

My [vimrc][] file is an edited version of "the ultimate vimrc file" found [here](http://amix.dk/vim/vimrc.html). A fitting title indeed.

Here are the settings which I've added or I find quite interesting or useful.

### Scroll using the spacebar ###

I added this to simulate smooth scrolling by pressing `<Space>` or `<Shift-Space>`, just like in a browser. 

{% highlight vim %}
map <S-Space> <C-Y><C-Y><C-Y><C-Y><C-Y><C-Y><C-Y><C-Y><C-Y><C-Y><C-Y>
map <Space> <C-E><C-E><C-E><C-E><C-E><C-E><C-E><C-E><C-E><C-E><C-E>
{% endhighlight %}

The smooth part only seems to work in GVim on Windows though. 

### Hotkeys ###

Some mappings to plugins and functions and other stuff which I use often. 

- `<F3>` is mapped to toggle NERDTree
- `<F4>` is mapped to toggle Taglist
- `<F5>` is mapped to insert the current timestamp
- `<leader>ev` opens up my .vimrc file for quick edits
- `<leader>x` is mapped to `:Bclose` to easily close files
- `<leader>cd` switch to the directory of the current file or buffer
- when editing certain filetypes (Java), `;` inserts a semicolon at the end of the line 

{% highlight vim %}
map <F3> :NERDTreeToggle<CR><CR>
map <F4> :TlistToggle<CR><CR>
nmap <F5> a<C-R>=strftime("%Y-%m-%d %I:%M:%S")<CR><Esc>
imap <F5> <C-R>=strftime("%Y-%m-%d %I:%M:%S")<CR>
map <leader>ev :e! ~/.vimrc<cr>
map <leader>cd :cd %:p:h<cr>
au FileType java silent noremap ; <Esc>mcA;<Esc>`c
{% endhighlight %}

### Switching between windows ###

Easily switch windows by pressing `<Ctrl>` + the direction of the window you want. 

{% highlight vim %}
map <C-j> <C-W>j
map <C-k> <C-W>k
map <C-h> <C-W>h
map <C-l> <C-W>l
{% endhighlight %}

### Toggle 80 Column Marker ###
I like to keep my Python code under 80 characters per line. And since the marker is a little distracting, I wanted an easy way to toggle it on or off.
I made a little function to do just that and mapped it to `<F2>` to quickly check my bounds.

{% highlight vim %}
" Toggle 80 column marker
nnoremap <F2> :call ToggleColorColumn()<CR>

func! ToggleColorColumn()
	if exists("b:colorcolumnon") && b:colorcolumnon
		let b:colorcolumnon = 0
		exec ':set colorcolumn=0'
		echo '80 column marker off'
	else
		let b:colorcolumnon = 1
		exec ':set colorcolumn=80'
		echo '80 column marker on'
	endif	
endfunc
{% endhighlight %}

You can now view my Vim setup on [Github][gitdotfiles] or [Bitbucket][hgdotfiles].

[Vim]: http://www.vim.org
[NERDTree]: http://www.vim.org/scripts/script.php?script_id=1658
[MiniBufExpl]: https://github.com/fholgado/minibufexpl.vim
[Bclose]: http://vim.wikia.com/wiki/Deleting_a_buffer_without_closing_the_window#Script
[Surround]: http://www.vim.org/scripts/script.php?script_id=1697
[surrounddocs]: https://github.com/tpope/vim-surround/blob/master/doc/surround.txt
[Repeat]: http://www.vim.org/scripts/script.php?script_id=2136
[Fugitive]: http://www.vim.org/scripts/script.php?script_id=2975
[SnipMate]: http://www.vim.org/scripts/script.php?script_id=2540
[LustyJuggler]: http://www.vim.org/scripts/script.php?script_id=2050
[Taglist]: http://www.vim.org/scripts/script.php?script_id=273
[vimrc]: {{site.github.url}}/media/uploads/vim/vimrc.html
[EasyMotion]: https://github.com/Lokaltog/vim-easymotion
[easymotionanimation]: https://d3nwyuy0nl342s.cloudfront.net/img/311e2034c078b3d7a53497020cda7b3bedda249d/687474703a2f2f6f6935342e74696e797069632e636f6d2f3279797365666d2e6a7067
[Pathogen]: http://www.vim.org/scripts/script.php?script_id=2332
[hgdotfiles]: https://bitbucket.org/john2x/dotfiles
[gitdotfiles]: https://github.com/john2x/dotfiles

