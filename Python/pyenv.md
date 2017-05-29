#pyenv
xcode-select --install mac 安装需要这个文件  

```
vi ~/.bash_profile
``` 
在其中添加 
 
```
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```
执行

```
source ~/.bash_profile 
``` 

global version  

