# shell-scripts-install-vim-wget
Which package we want to install 
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
Loop for all servers 
~~~
[root@ansible shell]# cat copy_install.sh
#!/bin/bash
cnt=1
for each_server in $(cat list_of_servers.txt)
do
echo "$cnt. working on ${each_server}"
scp viminstall.sh root@${each_server}:/tmp
ssh root@${each_server} "cd /tmp/ && ./viminstall.sh && echo -e '123456\n123456' | passwd root "
done

~~~
Server IP list
~~~
[root@ansible shell]# cat list_of_servers.txt
10.10.10.44
10.10.10.21
~~~

chmod +x copy_install.sh
chmod +x viminstall.sh

