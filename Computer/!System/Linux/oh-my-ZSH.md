## 安装

`sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

## 常用主题

`cd .oh-my-zsh/themes`

`cp robbyrussell.zsh-theme myrobbyrussell.zsh-theme`

```shell
PROMPT="%{$fg[red]%}%m%\ %{$fg[red]%}%n%\ "
PROMPT+=' %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[blue]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[blue]%} "
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

