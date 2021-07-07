
+++
title = "Ubuntu Shell环境配置"
date = "2021-07-02"
author = "xblzbjs"
tags = [
  "Linux",
  "Ubuntu",
]
+++

## 终端模拟器选择

推荐三个

### 1. [Gnome Terminal（原生）](https://help.gnome.org/users/gnome-terminal/stable/)

### 2. [Terminator（在用）](https://gnometerminator.blogspot.com/p/introduction.html)

### 3. [Guake](http://guake-project.org/)

## 终端配置（Zsh + Oh My Zsh + PowerLevel10k）

### 1. 安装[Zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

```zsh
# apt 打包安装 zsh
sudo apt install zsh
# 确认 zsh 成功安装
zsh --version
# 设置为默认Shell并确认
chsh -s $(which zsh)
echo $SHELL
```

### 2. 安装[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh)

```zsh
# 推荐使用curl安装
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)
# 如果没有安装curl，安装一下吧，你会发现它十分强大
sudo apt install zsh
```

### 3. 配置Zsh主题(PowerLevel10k)

Oh My Zsh自带的主题也很不错,你可以参考参考。

我自己的电脑上设置的主题为 [Powerlevel10k](https://github.com/romkatv/powerlevel10k)

```zsh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```

安装下面的字体文件:

- [MesloLGS NF Regular.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS)
- [MesloLGS NF Bold.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS)
- [MesloLGS NF Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS)
- [MesloLGS NF Bold Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS)

设置 `~/.zshrc`

```zsh
vim ~/.zshrc
```

设置 `ZSH_THEME="powerlevel10k/powerlevel10k"`，

```zsh
source ~/.zshrc
```

### 4. Oh My Zsh插件

目前在用的

- [git](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/git/git.plugin.zsh)
- [autojump](https://github.com/wting/autojump)
- [syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- [autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [blackbox](https://github.com/StackExchange/blackbox)

更多优秀的插件请参考

[awesome-zsh-plugins](https://project-awesome.org/unixorn/awesome-zsh-plugins)