---
title: VIM-插件配置
date: 2020-01-13 22:07:50
tags:
	- 操作系统配置
	- linux
	- VIM
---

### VIM 常用插件

#### VIM 插件管理器

- `vim-plug`是一个极简的vim插件管理器。

- 安装步骤

  - 首先安装curl。

  - 安装nodejs `curl -sL install-node.now.sh/lts | bash`。

  - `curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim` 安装vim-plug插件管理器。

  - 此时打开`vimrc`配置文件

    ~~~
    call plug#begin('~/.vim/plugged')
    “中间为要安装的插件
    Plug 'neoclide/coc.nvim', {'branch': 'release'}
    call plug#end()
    ~~~

  - 保存并退出vim，重新打开vim，输入`PlugInstall`，就会出现安装界面，耐心等待即可，出现`Finishing ... Done!`则表示安装成功。

  - 常用命令

    | 命令        | 功能                                   |
    | ----------- | -------------------------------------- |
    | PlugList    | 列出所有已配置的插件                   |
    | PlugInstall | 安装vimrc中plugged中列出来的插件       |
    | PlugClean   | 卸载没有在vimrc中plugged中列出来的插件 |
    | PlugUpdate  | 更新vimrc中plugged中列出来的插件       |

    

- 如何安装其他插件

  - 在GitHub上面找到想要安装的插件，例如`coc-nvim`，其url为`https://github.com/neoclide/coc.nvim`，插件名称是`neoclide/coc.nvim`，安装的时输入正确的名称即可。

    

#### 补全插件

- coc-nvim

  - 这个插件的安装很方便，使用plug安装。

  - `Plug 'neoclide/coc.nvim', {'branch': 'release'}`

  - 安装完成,打开vim，输入`CocConfig`进入补全配置界面。

    ```scheme
     {   
       "languageserver": {
           "clangd": {
             "command": "clangd",  
             "args": ["--background-index"],
             "rootPatterns": ["compile_flags.txt", "compile_commands.json", ".vim/", ".git/",".hg/"],
             "filetypes": ["c", "cpp", "objc", "objcpp"]
           }
         }
     }
    
    ```

    此处的配置是C/C++的补全配置，如果提示没有clangd，安装clangd即可。若需要配置其他语言的补全

  - [各语言补全脚本配置](https://github.com/neoclide/coc.nvim/wiki/Language-servers)

  - 也可以使用`languageserver`进行补全提示。

- YouCompleteMe

  - YouCompleteMe这个算是最难安装的插件不为过，网上的教程虽然很多，但是能成功的屈指可数，在我踩了无数的坑后，发现了一个可以一次性成功安装的办法。

    [YouCompleteMe插件安装](https://blog.csdn.net/tianminggenie/article/details/82899498)

    按照此博客的步骤，最终只需要配置`ycm_extra_conf.py`文件即可。

    ~~~
    找到 flags 字段，配置头文件目录。
    例如：
      '/usr/lib/gcc/x86_64-linux-gnu/8/',
      '-isystem',
      '/usr/include/',
      '-isystem',
      '/usr/lib/gcc/x86_64-linux-gnu/',
      '-isystem',
      '/usr/include/c++/7.5.0/',
      '-isystem',
      '/usr/local/include/',
    
    ~~~

#### 其他插件

- 括号补全插件 [auto-pairs](https://github.com/jiangmiao/auto-pairs)

- 新的vim启动界面，还支持打开最近打开的文件。 [vim-startify](https://github.com/mhinz/vim-startify)

- 状态栏 [vim-airline](https://github.com/vim-airline/vim-airline)

- 目录树 [nerdtree](https://github.com/preservim/nerdtree)

- 标签导航 [tagbar](https://github.com/majutsushi/tagbar)

- 标签列表  [taglist](https://github.com/vim-scripts/taglist.vim)

  

