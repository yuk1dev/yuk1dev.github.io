+++
date = '2025-01-03T22:44:10+08:00'
draft = false
title = 'Arch安装记录'
+++

#### 联网

- 无线

```bash
station wlan0 connect huaweif30
```



#### 分区



#### 如何安装双系统

- 先安装windows， 注意EFI分区需要分大点，如800M

https://support.microsoft.com/en-us/windows/create-installation-media-for-windows-99a58364-8c02-206f-aa6f-40c3b507420d#ID0EJD=Windows_10

- 直接在固态硬盘上继续新建分区
- 先正常安装arch的引导
- 进入arch后安装os-prober寻找windows系统
- 重新执行

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```





---

#### pacman 并行下载

```bash
vim /etc/pacman.conf
```

- 取消Parallel和Color的注释



#### 配置DHCP与DNS

- /etc/resolv.conf

```bash
nameserver 8.8.8.8
nameserver 114.114.114.114
```

- /etc/systemd/network/1.network

```bash
dhcp = yes
```



#### 开放 ssh 端口

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo systemctl restart iptables
sudo systemctl restart sshd
```

##### scp传输示例

```bash
scp /home/user/file.txt root@192.168.1.100:/tmp/
```



#### 中文字体

```bash
sudo pacman -S adobe-source-han-serif-cn-fonts adobe-source-han-sans-cn-fonts adobe-source-code-pro-fonts noto-fonts-cjk noto-fonts
```



#### 安装 yay

```bash
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```



#### 安装 clash-verge

```bash
yay -S clash-verge-rev-bin
```



#### 配置 zsh

```bash
sudo pacman -S zsh

chsh -s /bin/zsh

sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
##如果不成功，请执行下面两条命令，成功了就不需要做下面两条
wget 47.93.11.51:88/install_zsh.sh
bash install_zsh.sh

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

- 使用命令`vim .zshrc`打开.zshrc文件，找到`plugins=()`这一行，将zsh-syntax-highlighting添加进去

```Bash
plugins=(git zsh-syntax-highlighting)
```

1. 安装其他插件

```Bash
##命令自动补全插件
mkdir ~/.oh-my-zsh/plugins/incr
wget http://mimosa-pudica.net/src/incr-0.2.zsh -O ~/.oh-my-zsh/plugins/incr/incr.plugin.zsh
##命令自动推荐，根据历史记录
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
##目录自动跳转插件
sudo apt install autojump
( cd $ZSH_CUSTOM/plugins && git clone https://github.com/chrissicool/zsh-256color )
```

- 使用命令`vim .zshrc`，打开后在最后插入以下内容：

```Bash
#设置终端颜色，提示符，及上一条指令返回码提示
autoload -U colors && colors
PROMPT="%{$fg[red]%}%n%{$reset_color%}@%{$fg[blue]%}%m %{$fg[yellow]%}%1~ %{$reset_color%}%# "
RPROMPT="[%{$fg[yellow]%}%?%{$reset_color%}]"
# Useful support for interacting with Terminal.app or other terminal programs
[ -r "/etc/zshrc_$TERM_PROGRAM" ] && . "/etc/zshrc_$TERM_PROGRAM"
source ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions/zsh-autosuggestions.plugin.zsh
source /usr/share/autojump/autojump.sh
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
```

#### 修改默认shell

```bash
chsh -s /bin/zsh
```



## prasanthrangan 主题

- 高分屏配置2倍缩放

`monitor = ,highres,auto,2`

- 禁用  rainbow borders animation





#### 中文输入法

```bash
pacman -S fcitx5-im \
fcitx5-rime \
fcitx5-configtool \
fcitx5-material-color
```

- 雾凇拼音 rime-ice-git

> 环境变量

```config
env = QT_IM_MODULE,fcitx
env = XMODIFIERS,@im=fcitx
```

#### 安装firefox

- PR_END_OF_FILE_ERROR



#### 快捷键

- 将 caps大小写绑定 super

```config
input {
	kb_options = caps:super
}
```



#### Kitty 美化

> 切换kitty配色

```bash
kitty +kitten themes
```

> 查看已安装字体

```bash
fc-list
```

> 设置字体大小

```bash
font_size 16
```

https://github.com/practical-tutorials/project-based-learning?tab=readme-ov-file



#### neovim 

- astronvim



#### Zathura pdf阅读器



#### 使用 paru 替代 yay

```bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```



#### 安装QQ

- 缩放

```bash
~/.local/share/applications/slack.desktop
Exec=env LD_PRELOAD=/usr/lib/libcurl.so.3 /usr/bin/slack --force-device-scale-factor=1.5 %U
```



#### nvim 配置 C++ 环境

- nvim + coc + clangd

> lsp 的集成



#### wayland ime

> typora 无法使用中文输入法

```bash
code --enable-wayland-ime
```







---



## 备份与系统迁移

- ventory 盘
- zstd 打包
- pactree 与 graphviz可视化查看依赖

#### timeshift

- 安装 xorg-xhost 否则无法启动
