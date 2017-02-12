+++
draft = false
date = "2017-02-12T23:10:08+07:00"
title = "sublime text to vim"

+++

Bắt đầu chuyển sang dùng Vim làm editor chính một cách nghiêm túc sau nhiều lần thử và bỏ cuộc và quay trở về với Sublime Text.
Lý do chuyển sang dùng Vim lý do chính là vì sống hầu hết thời gian trong terminal nên di chuyển từ từ cho đồng bộ, lý do phụ là để làm màu và động lực để chuyển sang [bàn phím 60%](https://www.google.com.vn/search?q=60%25+keyboard&tbm=isch&tbo=u&source=univ&sa=X&ved=0ahUKEwjX9Oz3xIrSAhUHtJQKHZ4tDZkQsAQIKw&biw=1440&bih=737) để mang đi mang về cho tiện ._.

Còn về nguyên nhân bỏ cuộc là vì không quen với VIM được, do có nhiều thứ quá khác với SublimeText, chính xác là trên SublimeText có mà VIM lại không có, nên lần này mình sẽ setup toàn bộ những thứ bên SublimeText qua VIM hết.

# Cài đặt

## Install
Tuỳ theo nhu cầu bạn có thể chọn [Vim](http://www.vim.org/), [NeoVim](https://neovim.io/), hoặc [MacVim](https://github.com/macvim-dev/macvim) tuỳ thích.
Thế mình chọn cái nào à? Mình chọn [NeoVim](https://neovim.io/) nhé, vì một tính năng rất mạnh mà NeoVim đã support sẳn [TrueColour](https://gist.github.com/XVilka/8346728), à ờ, tóm lại cũng là màu mè thôi, cho nó đẹp.
Nếu bạn là newbie thì mình khuyến khích dùng `Vim` nhé, 

- [Vim](http://www.vim.org/download.php)
- [NeoVim](https://github.com/neovim/neovim/wiki/Installing-Neovim)
- [MacVim](https://github.com/macvim-dev/macvim/blob/master/README_vim.md#installation)


# Sử dụng Vim
Mở terminal và gõ `vim` để bắt đầu nào 

## Thao tác cơ bản 

### Mode

Vim có tổng cộng 6 modes hoạt động `normal`, `insert`, `visual`, `select`, `command-line`, `Ex-mode`. 
Mặc định khi mở `Vim` lên sẽ là mode `normal`.

Trong đó 3 modes mình thường được sử dụng nhất là:
- `normal`: Dùng để di chuyển và thao tác trong đoạn text, và sẽ là mode được back lại khi ấn `ESC` ở mode khác.
- `insert`: Như tên gọi, để thêm text mới vào.
- `visual`: tương tự như normal nhưng cho phép `bôi đen` (select) text để thực hiện các thao tác với đoạn `bôi đen` đó.

### Một vài phím và lệnh cơ bản

- [...]
- Sau khi viết một đoạn và thấy quá lười nên thôi mấy bạn xem bài của @huytd ở đây nha, hình ảnh đẹp và rõ ràng dễ hiểu [Làm quen VIM trong 5 phút](https://kipalog.com/posts/Lam-quen-VIM-trong-5-phut)

## Các công cụ & website hổ trợ học VIM nhanh hơn, vui hơn, thông minh hơn:
- Đầu tiên phải kể đến `của nhà trồng được`: `Tutor`. Mặc định trong Vim có một hệ thống Tutorial khá xịn, tại mode `normal` bạn gõ `:Tutor` và làm theo Tutorial nhé, sau khoảng 30' bạn có thể thoải mái thao tác trong Vim.
- Vừa học vừa chơi: [VIM Advantures](https://vim-adventures.com/) Một game trên nền web giúp bạn vừa học vừa chơi, di chuyển thao tác bằng các phím trong VIM. Bạn có thể chơi thử Level 1 rồi quyết định có mua license hay không (không free đâu nhé T_T)
- [OpenVim](http://www.openvim.com/) một website giúp bạn tập thao tác VIM online qua từng bài học, thực hành thẳng trên website.
- [Learn Vimscripts the Hard Way](http://learnvimscriptthehardway.stevelosh.com/) một ebook hướng dẫn các bạn nâng trình độ dùng VIM lên một tầm cao mới

    
## Tập sống khổ
Ban đầu mình vẫn dùng Sublime Text nhưng hạn chế không dùng đến các phím `arrow` và `pageUp/pageDown` và dùng các phím tắt trong VIM

- Đầu tiên mình cài thêm Plugin vào trong Sublime Text để làm quen dần thao tác di chuyển [Vintage](https://www.sublimetext.com/docs/2/vintage.html)(ST2) hoặc [Vintagous](https://github.com/guillermooo/Vintageous#vintageous)(ST3) 

- Cài thêm [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en) vào Chrome luôn cho ngầu.

# Di cư từ Sublime Text sang VIM
Phần sau đây xem như các bạn đã nắm được các thao tác cơ bản trên VIM rồi nhé.
Hầu hết các tính năng ở Sublime Text đều có thực hiện giống hoặc tốt hơn trong VIM nhờ vào hệ thống plugin đồ sộ cùng hệ thống có sẳn cho phép bạn tinh chỉnh nhiều thứ theo ý mình.
Và đều được định nghĩa trong file `.vimrc` (thường nằm ở `~/.vimrc`)

## Plugin management 
Để cài đặt Plugin trong VIM có thể cài trực tiếp bằng cách pull code từ github về và chép vào thư mục `~/.vim/bundle`. Tuy nhiên, cách tốt nhất vẫn là dùng hệ thống quản lý Plugin như [Vundle](https://github.com/VundleVim/Vundle.vim), [Pathegon](https://github.com/tpope/vim-pathogen), [Vim-Plug](https://github.com/junegunn/vim-plug), [Neo-Bundle](https://github.com/Shougo/neobundle.vim)
Cá nhân mình dùng `Vim-Plug` (trước đó dùng `Vundle`) vì `Vundle` hiện tại đã không còn maintain và `vim-plug` cũng đơn giản, dễ dùng và load, update cũng nhanh hơn hẳn.

- Install: https://github.com/junegunn/vim-plug#installation
- Usage: https://github.com/junegunn/vim-plug#usage
 - Thêm các plugins cần dùng trong file `.vimrc` 
 - Lưu lại và reload file `.vimrc` (có thể reload nhanh bằng command `:source %` mà không cần khởi động lại VIM)
 - `:PlugInstall` để cài đặt plugins
 - Ngoài ra `:PlugUpdate` để update toàn bộ plugins, hoặc `:PlugClean` để clean các plugins nào không còn nằm trong `.vimrc`, có thể xem thêm ở [đây](https://github.com/junegunn/vim-plug/wiki/tutorial)

## Cài đặt các thứ có sẳn trong VIM
Có rất nhiều người share config trong VIM trên github (và các dotfiles khác như tmux, zsh) mà các bạn có thể tham khảo để dùng cho bản thân, VD như [vimrc](https://github.com/amix/vimrc), [ThoughtBot](https://github.com/thoughtbot/dotfiles/blob/master/vimrc), [dotfiles](https://github.com/skwp/dotfiles)

Tuy nhiên: 

> Don't put any lines in your vimrc that you don't understand.

# Các vấn đề thường gặp khi bắt đầu dùng VIM
Các phần sau bạn có thể thử nhanh qua command `:` hoặc sửa trong `.vimrc` rồi reload lại VIM.

- Dùng chuột, hoàn toàn có thể:

```

if has('mouse')
  set mouse=a
endif

```

- Copy từ VIM ra ngoài Clipboard, mặc định VIM chỉ lưu trong Register ([xem thêm](http://vim.wikia.com/wiki/Accessing_the_system_clipboard)):

```
set clipboard=unnamedplus
```
- Hiện line number 

```
set nu
```

- Tự động cập nhật khi file thay đổi (bởi chương trình khác)

```
set autoread
set autowrite
```

- Tự động `indent`

```
set autoindent
set si "smart indent
```

- Bật syntax
```
syntax on
```

- Tắt các thể loại files backup, swap, khá là annoy và không cần thiết vì mình đã dùng `git` rồi

```
set nobackup
set nowb
set noswapfile
set backupdir=~/tmp,/tmp
set backupcopy=yes
set backupskip=/tmp/*,$TMPDIR/*,$TMP/*,$TEMP/*
set directory=/tmp
```

## Map key

Để có thể tận dụng hết tất cả sức mạnh của VIM cũng như thao tác một cách nhanh nhất thì không thể nào bỏ qua tính năng `map` - cho phép bạn sử dụng tổ hợp phím nhanh nhất, bạn có thể xem thêm [ở đây](http://vim.wikia.com/wiki/Mapping_keys_in_Vim_-_Tutorial_(Part_1))
Ví dụ mình thường `map` một số phím như 

```
" Map leader key
let mapleader = ","	" map leader key to ,
let g:mapleader = ","

" Fast saving
nmap <leader>w :w!<cr>

" Map Esc to jj
:imap jj <Esc>

" Hide highlight 
map <silent> <leader><cr> :noh<cr>

" Move between windows
map <C-j> <C-W>j
map <C-k> <C-W>k
map <C-l> <C-W>l
map <C-h> <C-W>h

```

Nói chung nếu những phím/tổ hợp phím/command nào thường xuyên sử dụng, bạn hoàn toàn có thể `map` lại thành phím khác để rút ngắn thời gian thao tác của mình.

# Nhưng vẫn chưa giống SublimeText?
- Làm thế nào để Split Screen đây? 

> Dùng command `:split` & `vsplit` để chia màn hình ngang và dọc

- Làm sao để di chuyển giữa các panes (màn hình)?

> Dùng chuột click cũng được, hoặc tốt nhất dùng tổ hợp phím `<C-W>j`, `<C-W>k`, `<C-W>l`, `<C-W>h`. `<C>` ở đây là phím `Ctrl`.

- VIM có SideBar folders không?

> Có plugin support nhé [NERDTREE](https://github.com/scrooloose/nerdtree)

- Vậy còn Mini map?

> Yup, [Minimap](https://github.com/severin-lemaignan/vim-minimap), nhưng cá nhân mình nghĩ có thứ tiện dụng hơn trong VIM là [Tagbar](https://github.com/majutsushi/tagbar)
