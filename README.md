# Implementação do serviço DNS MASTER

## Instalação

 * Instalar o bind9 via apt-get
```bash
$ sudo apt-get install bind9 dnsutils bind9-doc 
```
 * Verifique o status do serviço:
```bash
$ sudo systemctl status bind9
```
   * O servoço deve estar ativo e funcional, caso não esteja execute o comando:
```bash
$ sudo systemctl enable bind9
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/001.jpg?raw=true">


## Diretórios do bind
   * Os arquivos do bind ficam na no diretório **/etc/bind**. 
```bash
$ cd /etc/bind  //entra no diretório /etc/bind
$ ls /etc/bind
```
<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/001.jpg?raw=true">

```bash
$ ls -la
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/004.jpg?raw=true">

### Zonas

 * As zonas são especificadas em arquivos **db**. Sendo assim, devemos criar um diretório para armazendar os arquivos de zonas, que sera o diretório ***/etc/bind/zones***  
```bash
$ sudo mkdir /etc/bind/zones
```
#### Criar arquivos db
   * Criar o arquivo **db** no diretório ***/etc/bind/zones***.
   * No nosso caso o domínio/zona local será grupo6.turma913.ifalara.local. Assim o arquivo db será db.grupo6.turma913.ifalara.local
   
##### zona direta
   * o arquivo db.grupo6.turma913.ifalara.local conterá os nomes das máquinas do domínio grupo6.turma913.ifalara.local
   * Para isso, devemos fazer uma cópia do arquivo /etc/bind/db.empty
```bash
$ sudo cp /etc/bind/db.empty /etc/bind/zones/db.grupo6.turma913.ifalara.local 
```

##### zona reversa
   * vamos criar a zona reversa a partir do arquivo /etc/bind/db.127
```bash
  $ sudo cp /etc/bind/db.127 /etc/bind/zones/db.10.9.13.rev
```

   * Dessa forma, o arquivo **db.10.9.14.rev** conterá a zona reversa da rede 10.9.13.0. 
   
   ### Edição dos arquivos db:
   
   #### zona direta: db.grupo6.turma913.ifalara.local
   * edite o arquivo  **db.grupo6.turma913.ifalara.local** para adcionar as informações do seu domínio
   
```bash 
$ sudo nano grupo6.turma913.ifalara.local
```
   
   <img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/005.jpg?raw=true">
   
   #### Zona reversa: db.10.9.13.rev
   * edite o arquivo **db.10.9.13.rev** para adcionar as informações da zona reversa
   
```bash   
$ sudo nano db.10.9.13.rev
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/006.jpg?raw=true">

### Configuração do named.conf.local
   * Para ativar as zonas descritas nos arquivos **db** deve-se editar o arquivo de configuracão do bind para informar onde eles foram salvos. As zonas são adicionadas em **/etc/bind/named.conf.local**.
   
```bash
$ sudo nano /etc/bind/named.conf.local
```

<img src
