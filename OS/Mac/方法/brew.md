# 安装

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

# 代理

编辑~/.curlrc

```
socks5 = "127.0.0.1:1080"
```



# 换源

- 清华源(本人在用)

  ```bash
  # 替换brew.git
  cd "$(brew --repo)"
  git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
  
  # 替换homebrew-core.git
  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
  
  # 刷新源
  brew update
  ```

  

- 阿里云

  ```bash
  # 替换brew.git
  cd "$(brew --repo)"
  git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
  
  # 替换homebrew-core.git
  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
  
  # 刷新源
  brew update
  ```

  

- 腾讯源

  ```bash
  替换brew.git:
  cd "$(brew --repo)"
  git remote set-url origin https://mirrors.cloud.tencent.com/homebrew/brew.git
  
  替换homebrew-core.git:
  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.cloud.tencent.com/homebrew/homebrew-core.git
  
  # 刷新源
  brew update
  ```