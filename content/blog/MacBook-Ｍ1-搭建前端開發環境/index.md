---
title: MacBook Ｍ1 搭建前端開發環境
date: "2021-05-02T22:40:32.169Z"
description: 如何快速搭建 MacBook Ｍ1 的前端開發環境
---

## 前言

2017 MacBook pro 的風扇搞得像在坐飛機一樣...
受不了還是投入 M1 的懷抱了，選擇的是 MacBook Air 16G/512G。

但在安裝一些開發環境和軟體的時候還是和 Intel 版本有一些區別，
以下紀錄。

## 安裝順序
1. [iTerm2](#iterm2)
1. [Homebrew](#homebrew)
1. [Oh-My-Zsh](#oh-my-zsh)
    * [powerline](#安裝-powerline-字型)
    * [zsh-autosuggestions](#zsh-autosuggestions)
    * [zsh-syntax-highlighting](#zsh-syntax-highlighting)
1. [Visual Studio Code](#visual-studio-code)
    * [FiraCode](#安裝-firacode-字型)
1. [Nvm](#nvm)

**注：** M1默認使用 zsh ，故直接省略。

## iTerm2

用來取代原生終端機，最近也有比較新的 [Hyper](https://hyper.is) 。
筆者 iTerm2 和 Hyper 都使用過，還是覺得 iTerm2 順手點。

直接至官網下載安裝：https://iterm2.com/downloads.html 。

## Homebrew

優秀的 macOS 套件管理系統。
目前 Homebrew 已相容 m1 ，直接安裝即可。

#### 安裝

```console {.line-numbers}
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
**注1：** 與 Intel 版不同的是 ARM 安裝在 `/opt/homebrew` 路徑下，而非之前的 `/usr/local/Homebrew`。

**注2：** 通常完成後還需要安裝環境邊數，但筆者發現之後安裝 `Oh-My-Zsh` 時環境變數被覆蓋了，所以之後再設定即可。

**注3：** ```bew cask``` 安裝軟件時跳出 ```Error: Unknown command: cask``` ，搜尋問題發現 3.0.0 版本後 cask 取消了，直接 ```brew install``` 即可。

## Oh-My-Zsh

zsh 用的套件管理程式，包含許多美化主題和擴充功能等。

### 安裝 (使用 curl )

```console {.line-numbers}
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 設定

#### 打開 .zshrc 文件

```console {.line-numbers}
vi ~/.zshrc
```

#### 添加 Homebrew 環境變數

```console {.line-numbers}
export PATH="/opt/homebrew/bin:$PATH"
``` 

#### 隱藏使用者名稱(選用)

```console {.line-numbers}
export DEFAULT_USER=`whoami`
```

#### 修改主題(選用) 

```console {.line-numbers}
ZSH_THEME="agnoster"
```

#### Powerline 字型

##### 安裝

```
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```

##### 設定字型
    iTerm2 -> Perferences -> Profiles -> Text -> Font (搜尋 Powerline 相關字型後選擇)
![](https://i.imgur.com/OZvteL2.png)

#### oh-my-zsh 擴充 (選用) 

##### zsh-autosuggestions
輸入指令時會提示符合的歷史指令。

```console {.line-numbers}
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

##### zsh-syntax-highlighting
高亮 zsh 命令。

```console {.line-numbers}
git clone git://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

##### 生效配置

```console {.line-numbers}
zsh ~/.zshrc
```

### 成果

![](https://i.imgur.com/JWVZ5X5.png)

## Visual Studio Code

### 安裝主程式

```console {.line-numbers}
brew install --cask visual-studio-code
```

### 安裝 FiraCode 字型

```console {.line-numbers}
brew tap homebrew/cask-fonts
brew install --cask font-fira-code
```

### 安裝 code 命令
`Cmd + Shift + P` 輸入 `shell` 後案 `Enter` 。

![](https://i.imgur.com/rZHl5Nv.png)


### 設定 ( `Ctrl + ,` 開啟 settings.json 設定檔)

```json {.line-numbers}
{
    // 設定字型
    "editor.fontFamily": "Fira Code",
    "editor.fontSize": 14,
    "editor.fontLigatures": true,
    // 設定終端機字型
    "terminal.integrated.fontFamily": "Source Code Pro for Powerline",
    "terminal.integrated.fontSize": 14
}
```
### 成果

![](https://i.imgur.com/PFOub1d.png)

## Nvm

Node Version manager ， NodeJS 的版本管理工具，可同時安裝和管理多個 NodeJS 版本。 

> *May 2021：* 根據 nvm 的 [github](https://github.com/nvm-sh/nvm#macos-troubleshooting) ，目前 arm64 架構不支援 NodeJS 15.x 以下的版本，因此使用 Rosetta 來安裝 NodeJS，關於 Rosetta 的[介紹](https://support.apple.com/zh-tw/HT211861)。

### 安裝

這邊推薦安裝最新版本，可參考 [github](https://github.com/nvm-sh/nvm)。

```console {.line-numbers}
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

**注：** 不建議使用 Homebrew 安裝。

### 設定

```console {.line-numbers}
# 確認 nvm 版本
~ nvm --version
0.38.0

# 用 -x86_64 啟動 zsh
arch -x86_64 zsh

# 確認環境架構
~ arch
i386

# 在 x64 環境下編譯安裝 node
~ nvm install v14.15.4

# 確認 node 環境架構
~ node -p process.arch

# 返回 arm64
~ exit

# 確認環境架構
~ arch
arm64

# 使用剛剛安裝的 node
~ nvm use v14.15.4
Now using node v14.15.4 (npm v6.14.10)
```

## 結尾

希望以上紀錄可以幫助讀者節省在開發環境配置上遇到的一些問題。
雖然 M1 不免還是有許多軟體支援的問題，但相信還是會隨著時間慢慢改善。

最後一句話總結 M1 的使用心得，真的香。

## Reference

> https://formulae.brew.sh/
> https://ohmyz.sh/
> https://github.com/powerline/powerline
> https://formulae.brew.sh/cask/visual-studio-code
> https://github.com/tonsky/FiraCode/wiki/Installing
> https://github.com/nvm-sh/nvm