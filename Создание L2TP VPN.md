https://www.youtube.com/watch?v=k6woIbcYVzU

https://my.justhost.ru/
dianov.js@gmail.com
7D5g123b


92.114.54.103
Login: root  
Password: 7D5g123b

Обновляем систему
```bash
sudo apt update && sudo apt upgrade
```

Меняем пароль
```bash
passwd
```

Далее ставим strongswan для настройки ipsec:
```bash
apt install strongswan
```

Настраиваем
```bash
nano /etc/ipsec.conf
```

Настройки
```bash
config setup  
conn rw-base  
    fragmentation=yes  
    dpdaction=clear  
    dpdtimeout=90s  
    dpddelay=30s  
conn l2tp-vpn  
    also=rw-base  
    ike=aes128-aes192-aes256-sha1-sha256-modp1024-modp3072  
    esp=aes128-aes192-aes256-sha1-sha256-modp1024-modp3072  
    leftsubnet=%dynamic[/1701]  
    rightsubnet=%dynamic  
    mark=%unique  
    leftauth=psk  
    rightauth=psk  
    type=transport  
    auto=add
```

Далее добавим секретный ключ для ipsec:
```bash
nano /etc/ipsec.secrets
```
```bash
# This file holds shared secrets or RSA private keys for authentication.  
# RSA private key for this host, authenticating it to any other host  
# which knows the public part.  
%any %any : PSK "verysecretipseckey"
```

Включаем и запускаем службу:
```bash
systemctl enable strongswan-starter
systemctl start strongswan-starter
```

Далее нужно устновить l2tp сервер:
```bash
apt install xl2tpd
```

Правим конфиги:
```bash
nano /etc/xl2tpd/xl2tpd.conf
```

```bash
[global]  
port = 1701  
auth file = /etc/ppp/chap-secrets  
access control = no  
ipsec saref = yes  
force userspace = yes  
  
[lns default]  
exclusive = no  
ip range = 10.32.100.100-10.32.100.199  
hidden bit = no  
local ip = 10.32.100.1  
length bit = yes  
require authentication = yes  
name = l2tp-vpn  
pppoptfile = /etc/ppp/options.xl2tpd  
flow bit = yes
```

Правим опции:
```bash
nano /etc/ppp/options.xl2tpd
```
```
ms-dns 8.8.8.8  
ms-dns 192.168.1.1  
asyncmap 0  
auth  
crtscts  
lock  
hide-password  
modem  
mtu 1460  
lcp-echo-interval 30  
lcp-echo-failure 4  
noipx  
refuse-pap  
refuse-chap  
refuse-mschap  
require-mschap-v2  
multilink  
mppe-stateful
```

И добавляем пользователей:
```bash
nano /etc/ppp/chap-secrets
```

```bash
systemctl enable xl2tpd
systemctl start xl2tpd
```

Редиректы
```bash
nano /root/ipsec
```
```
iptables --table nat --append POSTROUTING --jump MASQUERADE  
echo 1 > /proc/sys/net/ipv4/ip_forward  
for each in /proc/sys/net/ipv4/conf/*  
do  
echo 0 > $each/accept_redirects  
echo 0 > $each/send_redirects  
done  
/etc/init.d/ipsec restart  
После этого надо сделать файл исполняемым и запустить скрипт:  
chmod +x /root/ipsec  
sh /root/ipsec
```
```bash
chmod +x /root/ipsec
sh /root/ipsec
```


Теперь вроде как сеть должна заработать как надо, но если ничего не предпринять, то после перезагрузки сервера – работоспособность будет нарушена. Нужно этот скрипт поместить в автозагрузку, для этого можно воспользоваться rc.d

```bash
nano /etc/systemd/system/rc-local.service
```
```
[Unit]  
 Description=/etc/rc.local Compatibility  
 ConditionPathExists=/etc/rc.local  
[Service]  
 Type=forking  
 ExecStart=/etc/rc.local start  
 TimeoutSec=0  
 StandardOutput=tty  
 RemainAfterExit=yes  
 SysVStartPriority=99  
[Install]  
 WantedBy=multi-user.target
```

Правим тут
```bash
nano /etc/rc.local
```