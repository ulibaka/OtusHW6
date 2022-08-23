1. Шарим директорию на сервере
[root@server ~]# mkdir /var/upload
[root@server ~]# chmod o+w /var/upload/
[root@server ~]# echo '/var/upload/ *(rw)' >> /etc/exports
[root@server ~]# exportfs -r
[root@server ~]# exportfs -s | grep upload
/var/upload  *(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
[root@server ~]# firewall-cmd --add-service=nfs   # требования для NFS: NFSv3 по UDP, включенный firewall.   
success
[root@server ~]# firewall-cmd --add-protocol=udp       
success


2. Монтируем директорию на клиенте

[root@client ~]# showmount --export 192.168.56.7 | grep upload
/var/upload   
[root@client ~]# mkdir /mnt/upload
[root@client ~]# mount -t nfs -o vers=3,proto=udp 192.168.56.7:/var/upload /mnt/upload/
[root@client ~]# grep upload /etc/mtab >> /etc/fstab 
[root@client ~]# tail -1 /etc/fstab 
192.168.56.7:/var/upload /mnt/upload nfs rw,relatime,vers=3,rsize=32768,wsize=32768,namlen=255,hard,proto=udp,timeo=11,retrans=3,sec=sys,mountaddr=192.168.56.7,mountvers=3,mountport=20048,mountproto=udp,local_lock=none,addr=192.168.56.7 0 0
[root@client ~]# touch /mnt/upload/check_file

# доп. задание не выполнил, не хватило знаний по кубику.

------Настроить аутентификацию через KERBEROS (NFSv4)-----

[root@server ~]# mkdir /var/nfsv4_with_krb5
[root@server ~]# echo '/var/nfsv4_with_krb5 *(rw)' >> /etc/exports
[root@server ~]# exportfs -r && exportfs -s | grep krb5
/var/nfsv4_with_krb5  *(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)

[root@server ~]#  yum install ipa-server -y
[root@server ~]#  ipa-server-install


