+++
draft = false
date = "2017-02-19T00:20:08+07:00"
title = "Di cư từ Sublime Text sang VIM"

+++

Bắt đầu chuyển sang dùng Vim làm editor chính một cách nghiêm túc sau nhiều lần thử, bỏ cuộc và quay trở về Sublime Text.
Còn về nguyên nhân bỏ cuộc là vì không quen với VIM được, do có nhiều thứ quá khác với SublimeText, chính xác là trên SublimeText có mà VIM lại không có, nên lần này mình sẽ setup toàn bộ những thứ bên SublimeText qua VIM hết.
# Vì sao phải dùng VIM?
1. Hầu hết thời gian mình đều làm việc trong terminal nên chuyển editor sang VIM luôn cho tiện.
2. Việc di chuyển trong VIM nhanh và thuận tiện hơn so với các editor/IDE khác (dĩ nhiên nếu không dùng VIM mode).
3. Hạn chế sử dụng chuột để tăng tốc độ thao tác.
4. Nhiều thứ để custom phù hợp với nhu cầu, khả năng tùy biến cao. 
5. Làm màu, trông có vẻ giống `hắc cơ.
6. Động lực để chuyển sang  [bàn phím 60%](https://www.google.com.vn/search?q=60%25+keyboard&tbm=isch&tbo=u&source=univ&sa=X&ved=0ahUKEwjX9Oz3xIrSAhUHtJQKHZ4tDZkQsAQIKw&biw=1440&bih=737) để mang đi mang về cho tiện ._.

# Cài đặt
Tuỳ theo nhu cầu bạn có thể chọn [Vim](http://www.vim.org/), [NeoVim](https://neovim.io/), hoặc [MacVim](https://github.com/macvim-dev/macvim) tuỳ thích.
Thế mình chọn cái nào à? Mình chọn [NeoVim](https://neovim.io/) nhé, vì một tính năng rất mạnh mà NeoVim đã support sẳn [TrueColour](https://github.com/neovim/neovim/wiki/FAQ#how-can-i-use-true-colors-in-the-terminal), à ờ, tóm lại cũng là màu mè thôi, cho nó đẹp.
Nếu bạn là newbie thì mình khuyến khích dùng `Vim` nhé.  

Link download & hướng dẫn cài đặt bên dưới nhé: 
- [Vim](http://www.vim.org/download.php)
- [NeoVim](https://github.com/neovim/neovim/wiki/Installing-Neovim)
- [MacVim](https://github.com/macvim-dev/macvim/blob/master/README_vim.md#installation)


# Sử dụng Vim
Mở terminal lên và gõ `vim` để bắt đầu nào (Windows thì bạn click vào cái icon sau khi cài đặt của nó nhé).

## Các vấn đề cơ bản 

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

## Học VIM nhanh hơn, vui hơn, thông minh hơn
1. Đầu tiên phải kể đến của nhà trồng được: `Tutor`. Mặc định trong Vim có một hệ thống Tutorial khá xịn, tại mode `normal` bạn gõ `:Tutor` và làm theo Tutorial nhé, sau khoảng 30' bạn có thể thoải mái thao tác trong Vim.
2. Vừa học vừa chơi: [VIM Advantures](https://vim-adventures.com/) Một game trên nền web giúp bạn vừa học vừa chơi, di chuyển thao tác bằng các phím trong VIM. Bạn có thể chơi thử Level 1 rồi quyết định có mua license hay không (không free đâu nhé T_T)
3. [OpenVim](http://www.openvim.com/) một website giúp bạn tập thao tác VIM online qua từng bài học, thực hành thẳng trên website.
4. [Learn Vimscripts the Hard Way](http://learnvimscriptthehardway.stevelosh.com/) một ebook hướng dẫn các bạn nâng trình độ dùng VIM lên một tầm cao mới
    
# Di cư từ Sublime Text sang VIM
Vấn đề đầu tiên khi chuyển sang VIM là cảm thấy quá nhiều thứ khác lạ và thiếu thốn, có mỗi cái màn hình đen chán phèo.
Bình tĩnh nhé, hầu hết các tính năng ở Sublime Text đều có thực hiện giống hoặc tốt hơn trong VIM nhờ vào hệ thống plugin đồ sộ cùng hệ thống có sẳn cho phép bạn tinh chỉnh nhiều thứ theo ý mình.
Và đều được định nghĩa trong file `.vimrc` (thường nằm ở `~/.vimrc`)

## Plugin management 
Có thể cài đặt Plugin trong VIM  trực tiếp bằng cách pull code từ github về và chép vào thư mục `~/.vim/bundle`. Tuy nhiên, cách tốt nhất vẫn là dùng hệ thống quản lý Plugin như [Vundle](https://github.com/VundleVim/Vundle.vim), [Pathegon](https://github.com/tpope/vim-pathogen), [Vim-Plug](https://github.com/junegunn/vim-plug), [Neo-Bundle](https://github.com/Shougo/neobundle.vim)
Cá nhân mình dùng [Vim-Plug](https://github.com/junegunn/vim-plug) (trước đó dùng `Vundle`) vì `Vundle` hiện tại đã không còn maintain, bên cạnh đó [Vim-Plug](https://github.com/junegunn/vim-plug) cũng đơn giản, dễ dùng, load, update plugins cũng nhanh hơn hẳn.

- **Cài đặt:** https://github.com/junegunn/vim-plug#installation
- **Cách dùng:** https://github.com/junegunn/vim-plug#usage
 - Thêm các plugins cần dùng trong file `.vimrc` 
 - Lưu lại và reload file `.vimrc` (có thể reload nhanh bằng command `:source %` mà không cần khởi động lại VIM)
 - `:PlugInstall` để cài đặt plugins
 - Ngoài ra `:PlugUpdate` để update toàn bộ plugins, hoặc `:PlugClean` để clean các plugins nào không còn nằm trong `.vimrc`, có thể xem thêm ở [đây](https://github.com/junegunn/vim-plug/wiki/tutorial)
![alt text](/static/img/sublime-text-to-vim/plug-in.gif)

## Dot file, .vimrc, config cho vim
Có rất nhiều người share config trong VIM trên github (và các dotfiles khác như tmux, zsh) mà các bạn có thể tham khảo để dùng cho bản thân, VD như [vimrc](https://github.com/amix/vimrc), [ThoughtBot](https://github.com/thoughtbot/dotfiles/blob/master/vimrc), [dotfiles](https://github.com/skwp/dotfiles)
Bạn có thể lên đây cóp nhặt các thứ cần thiết cho mình và ném nó vào file `.vimrc`, phần sau mình sẽ giới thiệu một số thứ thông dụng.

Tuy nhiên: 
> Don't put any lines in your vimrc that you don't understand.

# Các vấn đề thường gặp khi bắt đầu dùng VIM
Các phần sau bạn có thể thử nhanh qua command `:` hoặc sửa trong `.vimrc` rồi reload lại VIM. Có thể check nhanh giá trị của biến bằng command `:set <variable>?`, vd `:set nu?`

- Dùng chuột, hoàn toàn có thể:

```
if has('mouse')
  set mouse=a
endif
```

- Copy từ VIM ra ngoài Clipboard, mặc định VIM chỉ lưu trong Register ([xem thêm](http://vim.wikia.com/wiki/Accessing_the_system_clipboard)) lúc bạn `yank`:

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
**Q:** VIM cùi bắp, không có auto complete này.
**A:** Vim có nhiều Plugin support auto-complete như [YouCompleteMe](https://github.com/Valloric/YouCompleteMe), [neocomplete](https://github.com/Shougo/neocomplete.vim)  ![alt text](/static/img/sublime-text-to-vim/youcompleteme.gif)
**Q:** Làm thế nào để Split Screen đây? 
**A:** Dùng command `:split` & `vsplit` để chia màn hình ngang và dọc. [Xem thêm](https://robots.thoughtbot.com/vim-splits-move-faster-and-more-naturally) để làm việc nuột hơn nà.

**Q:** Làm sao để di chuyển giữa các panes (màn hình)?
**A:** Dùng chuột click cũng được, hoặc tốt nhất dùng tổ hợp phím `<C-W>j`, `<C-W>k`, `<C-W>l`, `<C-W>h`. `<C>` ở đây là phím `Ctrl`.

**Q:** VIM có SideBar folders không?
**A:** Có plugin support nhé [NERDTree](https://github.com/scrooloose/nerdtree)
![alt text](/static/img/sublime-text-to-vim/nerdtree.png)

**Q:** Thêm/xóa file/thư mực như thế nào nhỉ, phải switch ra Terminal à?
**A:** Không cần phải ra terminal đâu, ở NERDTree bạn chỉ việc ấn phím `m` tại node cần thao tác, một menu sẽ hiện ra cho bạn chọn lựa
![alt text](/static/img/sublime-text-to-vim/nerdtree_menu.png)

**Q:** Vậy còn Mini map?
**A:** Yup, [Minimap](https://github.com/severin-lemaignan/vim-minimap), nhưng cá nhân mình nghĩ dùng [Tagbar](https://github.com/majutsushi/tagbar) sẽ tiện dụng hơn.
Minimap:![alt text](/static/img/sublime-text-to-vim/minimap.gif)
Tagbar:![alt text](/static/img/sublime-text-to-vim/tagbar.png)
**Q:** Mình hay dùng Mutitple Selection.
**A:** Xem nào, Multiple Selection có nhiều thứ, ta đi qua từng cái nhé:
1. Thêm một hàng: Mặc định ở Windows là `Ctrl+Alt+Up` & `Ctrl+Alt+Down` (OS X: `Ctrl+Shift+Up` & `Ctrl+Shift+Down`).
**A:** Đối với VIM, khi ở mode Normal có thể ấn `o` để thêm một hàng phía dưới và `O` để thêm phía trên và sau đó sẽ switch sang mode Insert luôn để bạn có thể bắt đầu gõ. Ngoài ra bạn có thể gõ `5o<Esc>` để thêm nhanh 5 hàng, tính năng repeat tiện lợi của VIM.

2. Chọn, sửa nhanh nhiều vị trí: `Ctrl+d` hoặc `Command+d`
**A:** VIM thì mình dùng Plugin [vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors) nhé, ngoài chức năng như Sublime Text bạn còn có thể search được bằng cả Regex
![alt text](/static/img/sublime-text-to-vim/multicursor.gif)
3. Select all bằng `Alt+F3` (Windows+Linux) hoặc `Ctrl+Command+g` trên OS X?
**A:** Vẫn dùng Plugin bên trên nha, bạn xem trong wiki để tìm hiểu thêm.

**Q:** Còn tab thì sao?
**A:** Hiển nhiên rồi, ngoài ra VIM còn có thể một khái niệm nữa là Buffer, xem thêm tab [ở đây](http://vim.wikia.com/wiki/Using_tab_pages) và buffer [ở đây](http://vim.wikia.com/wiki/Buffers)
![alt text](/static/img/sublime-text-to-vim/tab.gif)
**Q:** Navigation bằng Ctrl + P thì sao, siêu tiện luôn
**A:** VIM có nhiều Plugin phục vụ cho việc này, bạn có thể dùng hàng Việt Nam chất lượng cao [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) hoặc dùng [Fzf](https://github.com/junegunn/fzf.vim)
![alt text](/static/img/sublime-text-to-vim/fzf.gif)
**Q:** Search một từ trong nhiều file? Ví dụ seach chữ `function` trong một project có nhiều thư mục, Sublime Text thì click phải vào Folder rồi search, còn VIM thì sao, đâu có click phải vào NERDTree được?
**A:** VIM bạn có thể search bằng built-in của VIM là `:grep`, `:lgrep`, `:vimgrep`, `:lvimgrep` http://vim.wikia.com/wiki/Find_in_files_within_Vim, cá nhân mình dùng [ack.vim](https://github.com/mileszs/ack.vim) kết hợp cùng [the_silver_searcher](https://github.com/ggreer/the_silver_searcher)
![alt text](/static/img/sublime-text-to-vim/ack.gif)

**Q:** Ops, không ấn `Ctrl + /` để comment code như các IDE khác à?
**A:** Để comment code bạn cần dùng Plugin [NERD Commenter](https://github.com/scrooloose/nerdcommenter) và sau đó có thể map phím `Ctrl + /` để execute lệnh comment

**Q:** Các tổ hợp phím tắt thì sao nhỉ? Ví dụ như mình muốn copy nhanh toàn bộ code trong một tag hoặc function?
**A:** Ở mode Normal gõ `yip` - copy bên trong tag hoặc `yap` copy cả tag, ngooài ra có rất nhiều phím tắt khác, để thao tác nhanh nhất bận nên tham khảo vim cheatsheet.

Những phần bên trên chỉ là phần nhỏ thôi, bạn có thể xem thêm trong `:help` hoặc tự map các tổ hợp phím tắt riêng cho mình giúp tăng tốc độ làm việc.

## Shock văn hóa, quá nhiều thứ để nhớ, thôi nghỉ, không VIM nữa
Ban đầu mình cũng không thể chuyển sang VIM ngay được, không thể nào thao tác quen được. Nếu quyết định chuyển từ từ sang, vẫn dùng Sublime Text nhưng hạn chế không dùng đến các phím `arrow` và `pageUp/pageDown` thay vào đó là dùng các phím tắt trong VIM

- Đầu tiên mình cài thêm Plugin vào trong Sublime Text để làm quen dần thao tác di chuyển [Vintage](https://www.sublimetext.com/docs/2/vintage.html)(ST2) hoặc [Vintagous](https://github.com/guillermooo/Vintageous#vintageous)(ST3) 

- Cài thêm [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en) vào Chrome luôn cho ngầu. Install xong thì ấn `?` để xem danh sách phím tắt nhé. 

![alt text](/static/img/sublime-text-to-vim/chrome.gif)

Rất nhanh và tiện, hoàn toàn thao tác bằng phím, không rờ tới chuột.

# Tiếp theo thì làm gì?
- Tùy theo ngôn ngữ mà bạn đang dùng mà bạn có thể cài thêm cái plugin hổ trợ để viết code nhanh hơn, tốt hơn, ít bug hơn.
- Nếu chức năng nào đó mà bạn quen dùng nhưng không có hãy mạnh dạn tùy biến `vimrc` hoặc `Google` thử, hầu hết sẽ có thứ bạn cần.
- Sao khi đi thao tác nhanh rồi thì việc tiếp theo là đi khè mấy đứa khác thôi.
Chúc các bạn may mắn


Triet 18-02-2017
