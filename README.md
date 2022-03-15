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
SSH Configure and Root Password Changed
~~~
[root@ansible shell]# cat sshenable.sh
#!/bin/bash
sed -i 's/PermitRootLogin no/PermitRootLogin yes/g' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd && systemctl status sshd
echo -e '1122334455\n1122334455' | passwd root
~~~
SSH-COPY-ID PUSH Remote Location
~~~
[root@ansible shell]# cat sshkeypush.sh
#!/bin/bash
cnt=1
pwd=1122334455
for each_server in $(cat list_of_servers.txt)
do
echo "$cnt. working on ${each_server}"
sshpass -p $pwd ssh-copy-id  root@${each_server}
done
~~~
Excutable permission 
~~~
chmod +x copy_install.sh
chmod +x viminstall.sh
chmod +x sshenable.sh
chmod +x sshkeypush.sh
~~~

