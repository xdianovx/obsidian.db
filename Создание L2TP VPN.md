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

```