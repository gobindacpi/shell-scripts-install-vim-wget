# shell-scripts-install-vim-wget
~~~
[root@ansible shell]# cat viminstall.sh
#!/bin/bash
vim --version 1>/dev/null 2>&1
if [[ $? -eq 0 ]]
then
echo "Already vim installed"
else
sudo yum install vim -y
fi
wget --version 1>/dev/null 2>&1
if [[ $? -eq 0 ]]
then
echo "Already wget installed"
else
sudo yum install wget -y
fi

~~~
