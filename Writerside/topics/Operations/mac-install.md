# mac

## mac安装nvm
```Shell
brew install nvm
echo "source $(brew --prefix nvm)/nvm.sh" >> .bash_profile
. ~/.bash_profile
```

## mac配置jdk多版本
```Shell
# brew install jenv
# brew install openjdk@11

echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(jenv init -)"' >> ~/.bash_profile
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc

source ~/.bash_profile

# 把homebrew安装的openjdk17软链接到系统目录，会复制到指定目录里面
sudo ln -sfn $(brew --prefix openjdk)/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk 
sudo ln -sfn $(brew --prefix openjdk@11)/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk@11.jdk 

jenv add /Library/Java/JavaVirtualMachines/openjdk@11.jdk/Contents/Home

echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile

source ~/.bash_profile
```

## brew命令
