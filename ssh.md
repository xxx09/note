# 问题描述


nano ~/.bashrc

# 在文件末尾添加以下内容：
# 保存原始 LD_LIBRARY_PATH
```
export ORIG_LD_LIBRARY_PATH=$LD_LIBRARY_PATH
```

# 创建 SSH 别名，使用系统库
```
alias ssh='LD_LIBRARY_PATH= /usr/bin/ssh'
alias scp='LD_LIBRARY_PATH= /usr/bin/scp'
alias sftp='LD_LIBRARY_PATH= /usr/bin/sftp'
alias ssh-keygen='LD_LIBRARY_PATH= /usr/bin/ssh-keygen'
```

[链接文字](https://example.com)  
·

查看某个东西在系统中可能存在的路径
sudo find / -name 'libcrypto.so*' 2>/dev/null

或者直接使用路径顺序
export LD_LIBRARY_PATH="/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu:${LD_LIBRARY_PATH}"

# 应用更改
`source ~/.bashrc`


方式二：将令牌嵌入远程URL（一劳永逸）
这种方法会将令牌直接写入仓库的远程地址配置，之后推送就无需再输入令牌了。
git remote set-url origin https://<your_token>@github.com/<username>/<repository>.git

