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

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/007.jpg?raw=true">

### Verificação de sintaxe 
   * Para checar a sintaxe de configuração do BIND deve-se executar o comando named-checkconf. Este scritp checa os arquivos /etc/bind/named.conf.local.*

```bash
$sudo named-checkconf
```
<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/10.jpg?raw=true">

###  Verificar a sintaxe dos arquivos de dados
   * Para verificar se a formatação da sintaxe dos arquivos db está correta, utiliza-se o script named-checkconf da seguinte forma: ***named-check-zone <zone> <db_file>***
   
   ```bash
$ cd /etc/bind/zones
$ sudo named-checkzone grupo6.turma913.ifalara.local db.grupo6.turma913.ifalara.local
```
<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/008.jpg?raw=true" >

```bash
$ sudo named-checkzone 13.9.10.in-addr.arpa db.10.9.13.rev
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/009.jpg?raw=true" >

### Configure para somente resolver endereços IPv4

```bash
$sudo nano /etc/default/named
```
- adicione a linha ***OPTIONS="-4 -u bind"***

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/011.jpg?raw=true">

### Execute o BIND 
```bash
$ sudo systemctl enable bind9
$ sudo systemctl restart bind9
```

### Configuração dos clientes
   * Configure o dns no nas máquina ns1, ns2 e us adicionando os campos abaixo na interface de rede local deses servidores. Observe que na máquina gw essa configuração deve ser inserida na interface de rede local (enp0s8)
   * O arquivo de configuração do netplan ficará da seguinte forma:
   
   <img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/012.jpg?raw=true">
   
   ### Testando o servidor DNS:

#### Teste de configuração como cliente. 
   * Observe se os campos **DNS servers** e **DNS Domain** estão corretos.
```bash
$ systemd-resolve --status enp0s3
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/013.jpg?raw=true">

#### Teste o serviço DNS para a máquina ns1.

```bash
$ dig ns1.grupo6.turma913.ifalara.local
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/014.jpg?raw=true"> 

#### Teste o serviço DNS reverso para a máquina ns1. 
```bash    
$ dig -x 10.9.14.10
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/015.jpg?raw=true">

#### Teste o serviço DNS reverso para a máquina ns2. 
```bash  
$ dig -x 10.9.14.11
```

<img src = "https://github.com/amaurisilva1340/NS1_labredes/blob/Images/016.jpg?raw=true"
