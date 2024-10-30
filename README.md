# raspberry-matzieb-installation

### Dla raspberry pi4, 1GB
System operacyjny: Raspberry Pi OS (32-bit) with desktop - gdy potrzebny, interfejs graficzny

### ustawienie autologowania dla trybu graficznego

```
/etc/lightdm/lightdm.conf
```

W tym pliku dodać tekst 
```
[Seat:*]
autologin-user=nazwa_użytkownika
```



### raspberry tekstowe
Tworzenie użytkownika i autologin

```
/etc/systemd/system/getty@tty1.service.d/autologin.conf


[Service]
ExecStart=
ExecStart=-/sbin/agetty --autologin nazwa_uzytkownika --noclear %I $TERM
```

### włączanie połączenia ssh
W katalogu boot stworzenie pustego pliku ssh
```
touch ssh
```

### sprawdzanie IP
```
nmap -sn 192.168.1.0/24
```


### kopiowanie publicznego klucza
```
ssh-copy-id name
```


### przygotowanie prompta
```
case $(hostname) in
    laptop*)
        prompt_color='\033[48;5;16m\033[38;5;46m'    # zielony (46) na czarnym (16) dla laptopa
        ;;
    ras4-prototype-1GB)
        prompt_color='\033[48;5;16m\033[38;5;208m'   # pomarańczowy (208) na czarnym (16) dla prototypu
        ;;
esac

ORIG_PS1=$PS1
if [ -z "${CUSTOM_HOST_NAME}" ]; then
    CUSTOM_HOST_NAME=$(hostname -s)
fi
PS1='\['${prompt_color}'\]\u@'${CUSTOM_HOST_NAME}'\[\033[m\]:\W\$ '
unset prompt_color
      
