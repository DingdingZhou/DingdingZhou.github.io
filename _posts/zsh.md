cd ~

git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

～目录下没有.zshrc
3.1   touch .zshrc
3.2   cp ~/.zshrc   ~/.zshrc.orig

创建zsh配置文件
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

配置兼容bash环境变量
.zshrc 文件中添加source ~/.bash_profile

修改默认shell
chsh -s /bin/zsh

修改zsh主题
.zshrc 文件ZSH_THEME=gnzh



Zsh在安装了oh-my-zsh以后，只需要在plugins那里添加vi-mode，然后在终端执行source ~/.zshrc或者是重启终端就开启vi-mode了。 

最后附上Bash在vi-mode下的快捷键bash-vi-editing-mode-cheat-sheet，基本上与vim是一致的。