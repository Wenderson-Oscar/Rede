 <center> 

 # COMANDOS DE CONFIGURAÇÃO DE SERVIDOR

 </center>

 ### Professor: Karl 
 ### Disciplina: Rede

<hr>

   * [Tutorial](#tutorial)

<hr>

### Exibir configuração de rede

```
ip addr

```

### Interface de rede
 <hr>

```
nano /etc/network/interface

```
**Alteração de arquivo**
```mermaid
# The primary network interface
# allow-hotplug eno1
# iface eno1 inet dhcp

# Static IP address
auto eno1
iface eno1 inet static
    addres x.x.x.x
    netmask x.x.x.x
    network x.x.x.x
    broadcast x.x.x.x
    gateway x.x.x.x
```

<hr>

### Configurando SSH

```
apt install openssh-server openssh-client -y

apt install bind9 bind9-doc dnsutils -y

apt install apache2 apache2-doc -y

nano /etc/ssh/sshd_config

```

**Alteração de arquivo**

```mermaid
Port 22
#AddressFamily any
ListenAddress x.x.x.x

PermitootLogin prohibit-password

PermitEmptyPasswords no
```

### Continuação de Configuração do SSH

```
systemctl restart ssh
```
<hr>

### configuração de Rede no Virtual box -- Interface app

**Download PuTTY**

Visit https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

<br>

**Vídeo de demostração (Configuração)**

Visit https://drive.google.com/file/d/1khRr6PvrTKNdUpjRh51IWll0JvOdV0iw/view

<hr>

### Já quando estiver tendo acesso remoto pelo servidor

```
nano /etc/bind/named.conf.local
```

**Alteração de arquivo**

Informamos nossas zonas e localização 

```
zone "agronet.com"{
    type master;
    file "/etc/bind/db.agronet.com";
};

#O ip e Interveso

zone "1.168.192.in-addr.arpa"{
    type master;
    file "/etc/bind/db.1.168.192";
};
```
<hr>

### Agora, configurar a resolução de nomes

```
nano /etc/bind/db.agronet.com
```

**Alteração de arquivo que sera feita no arquivo em 'branco'**

```
; BIND zone para agronet.com

$TTL    3D
@       in      SOA     @               root.agronet.com.(
                        2022121301      ; serial
                        8H              ; refresh
                        2H              ; retry
                        4w              ; expire
                        1D )            ; minimum

@               NS      ns
@               MX      10      mail

ns              A       192.168.1.2
mail            A       192.168.1.2

agronet.com.    A       192.168.1.2
agronet         A       192.168.1.2

```

**Em Seguida**

```
nano /etc/bind/db.1.168.192
```

**Alteração de arquivo que sera feita no arquivo em 'branco'**

```
; BIND zone 

$TTL    3D
@       in      SOA     @               root.agronet.com.(
                        2022121301      ; serial
                        8H              ; refresh
                        2H              ; retry
                        4w              ; expire
                        1D )            ; minimum

@               NS      ns.agronet.com.

2     PTR     agronet.agronet.com.
2     PTR     ns.agronet.com.
2     PTR     mail.agronet.com.
1     PTR     router.agronet.com.
```

**Logo em Seguida**

```
systemctl restart bind9
```

**Comando de Teste de zona**

```
nslookup agronet.com
nslookup ns.agronet.com
nslookup 192.168.1.2
```

<hr>

# Tutorial

depois que entrarmos no sistema pela virtual box...

### Teremos que baixar algumas dependencias

```
apt install openssh-server openssh-client -y

apt install bind9 bind9-doc dnsutils -y

apt install apache2 apache2-doc -y
```

### Em seguida editaremos o arquivo de interface

```
nano /etc/network/interface

```
### Exibir configuração de rede

```
ip addr

```

### Iremos fazer algumas modificações

```
iface enp0s3 inet static
    addres x.x.x.x #ip 2
    netmask x.x.x.x #mascara
    gateway x.x.x.x #ip 1
    network x.x.x.x #ip 0
    broadcast x.x.x.x #ip 63
```

### Comando de configuração de ssh

```
nano /etc/ssh/sshd_config
```

### Iremos desmarcar algumas coisas

```
#Port 22
#ListenAddress 192.168.1.2
#PermitRootLogin prohibit-password
#PermitEmptyPasswords no

```
### Agora iremos reiniciar 

```
systemctl reboot
```

### Putty

**Download PuTTY**

Visit https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html


### Iremos configurar a rede da maquina

**Vídeo de demostração (Configuração)**

Visit https://drive.google.com/file/d/1khRr6PvrTKNdUpjRh51IWll0JvOdV0iw/view

### Ainda no Putty

```
nano /etc/bind/named.conf.local
```

**Iremos adicionar no arquivo**

```
zone "agronet.com"{
    type master;
    file "/etc/bind/db.agronet.com";
};

#O ip e Interveso

zone "1.168.192.in-addr.arpa"{
    type master;
    file "/etc/bind/db.1.168.192";
};
```
```
nano /etc/bind/db.agronet.com
```

**No arquivo em 'branco' iremos adicionar os seguinte dados**

```
; BIND zone para agronet.com

$TTL    3D
@       in      SOA     @               root.agronet.com.(
                        2022121301      ; serial
                        8H              ; refresh
                        2H              ; retry
                        4w              ; expire
                        1D )            ; minimum

@               NS      ns
@               MX      10      mail

ns              A       192.168.1.2
mail            A       192.168.1.2

agronet.com.    A       192.168.1.2
agronet         A       192.168.1.2

```
**Em Seguida**

```
nano /etc/bind/db.1.168.192
```

**Alteração de arquivo que sera feita no arquivo em 'branco'**

```
; BIND zone 

$TTL    3D
@       in      SOA     @               root.agronet.com.(
                        2022121301      ; serial
                        8H              ; refresh
                        2H              ; retry
                        4w              ; expire
                        1D )            ; minimum

@               NS      ns.agronet.com.

2     PTR     agronet.agronet.com.
2     PTR     ns.agronet.com.
2     PTR     mail.agronet.com.
1     PTR     router.agronet.com.
```
**agora**

```
systemctl reboot
```

```
nano /etc/resolv.conf
```

iremos modificar o domain, search, nameserver

```
domain agronet.com
search agronet.com
nameserver 192.168.1.2
```

**Comando de Teste de zona**

```
nslookup agronet.com
nslookup ns.agronet.com
nslookup 192.168.1.2
```