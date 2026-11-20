# Slackware Scripts 🐧

Este repositório é o meu canivete suíço pessoal para o Slackware.
Uma coleção direto ao ponto de scripts para automatizar qualquer tarefa,
configuração ou personalização do sistema.

## 🌐 Como configurar a Rede no Setup do Slackware

Como o ambiente de boot do Slackware não possui Git instalado nativamente,
você deve ativar a rede no terminal e usar o `wget` para baixar os arquivos
ou usar o `ssh` para tranferir os arquivos.

### Como Ativar a Rede no Instalador

Para ver o nome da sua Internet:
```bash
ip link
```

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 08:00:27:e1:4c:5f brd ff:ff:ff:ff:ff:ff
```

Para ativar a conexão via dhcp, que nesse exemplo é eth0:
```bash
dhcpcd eth0
```

Testar a conexão usando ping:
```bash
ping -c4 8.8.8.8
```

Se quiser tranferir via SSH:
```bash
/etc/rc.d/rc.dropbear start
```

### Baixar Qualquer Script Pela Web Usando `wget`

Para baixar qualquer script deste repositório usando o `wget`, use a URL do
arquivo no modo **Raw**. Útil se você não tiver outro PC para transferir os arquivos.

Estrutura do comando:
```bash
wget https://raw.githubusercontent.com/LinuxProativo/SlackScripts/master/<SCRIPT>
```

Exemplo prático de execução:
```bash
wget https://raw.githubusercontent.com/LinuxProativo/SlackScripts/master/btrfs-uefi
chmod +x btrfs-uefi
./btrfs-uefi
```

### Transferir Arquivos Usando SSH

Primeiramente, clonar o repositório usando outro PC:
```bash
git clone https://github.com/LinuxProativo/SlackScripts.git
```

Para consultar o ip dentro do ambiente de boot do Slackware, exemplo acima eth0:
```bash
ip addr show eth0 | grep "inet"
```

```
inet 192.168.0.10/24 brd 192.168.0.255 scope global deprecated dynamic noprefixroute eth0
inet6 [ ... ] # só pra exemplo
```

Transferindo o diretório inteiro para o boot do Slackware (exige iniciar o rc.dropbear):
```bash
tar -czf - SlackScripts/ | ssh root@192.168.0.10 "tar -xzf - -C /"
```

Ou transferindo só um arquivo:
```bash
tar -czf - SlackScripts/btrfs-uefi | ssh root@192.168.0.10 "tar -xzf - -C /"
```

Para acessar via ssh, para qualquer intervenção manual:
```bash
ssh root@192.168.0.10
```

Ao pedir a senha só de Enter direto porque no boot de instalação do Slackware não tem.

### Usando um Pendrive Montado

Prepare um pendrive com os arquivos. Depois, entre no boot de instalação do Slackware.

Verificar o pendrive conectado:
```bash
lsblk -SQ "TRAN == 'usb'"
```
 
```
NAME HCTL       TYPE VENDOR   MODEL         REV SERIAL               TRAN
sdb  2:0:0:0    disk SanDisk  Cruzer Blade 1.00 03003416122920233219 usb
```

Ou apenas:
```bash
lsblk -S                 
```

```bash
NAME HCTL       TYPE VENDOR   MODEL          REV SERIAL               TRAN
sda  1:0:0:0    disk ATA      VBOX HARDDISK  1.0 VB44664c1c-f6c932e8  sata
sdb  2:0:0:0    disk SanDisk  Cruzer Blade  1.00 03003416122920233219 usb
sr0  0:0:0:0    rom  VBOX     VBOX CD-ROM    1.0 VB0-1a2b3c4d         sata
```

Montando o pendrive:
```bash
mkdir -p /mnt/usb
mount /dev/sdb1 /mnt/usb
```

### Extra: Servidor Local

Isso também funciona só que é menos prático.

```bash
ip addr show eth0 | grep "inet"
inet 192.168.0.09/24 [ ... ] eth0 # Exemplo de saída
```
```bash
cd SlackScripts
php -S 0.0.0.0:8080          # Alternativa PHP
python3 -m http.server 8080  # Alternativa Python
```

No boot de instalação do Slackware:
```bash
wget -c http://192.168.0.09:8080/btrfs-uefi
```

## 📝 Lista de Scripts Disponíveis

* `auto-chroot-btrfs` - Auto chroot automático para rodar no modo LiveCD ou ambiente de
  instalação para montar e acessar facilmente sistemas instalados em Btrfs com subvolume.
* `btrfs-uefi-setup` - Setup custom para tabela de partição GPT, partição UEFI,
  formatação em Btrfs com subvolumes `@` e `@home`, swap ou swapfile opcional, `fstab`
  otimizado, subvolume `@tmp` ou `/tmp` na RAM opcional, configuração de idioma, teclado,
  variável PATH, serviço NTP apenas para pt_BR, criação de usuário com grupos adicionais,
  ativação do sudo, initdefault runlevel 4, geração de initrd para Btrfs, instalação do
  `GRUB` com e demais configurações.
* `f2fs-format` - Automatiza a formatação F2FS em discos ou partições, com flags modernas
  de integridade e adição de entrada no `fstab` existente (suporte a Live/Chroot).
* `install-pkgs-alien` - Instala alguns pacotes do repositório `alien` para o Slackware
  e esse pode ser ajustado para adicionar pacotes conforme a necessidade.
* `pkg-clean` - Automatiza a limpeza de pacotes por categorias, principalmente os jogos
  do **`KDE GAMES`** que eu nem sei se tem alguém que joga.
* `pos-config-ptbr` - Pós-configuração convencional em português pra quem instalou o
  Slackware pelo setup padrão. Inclui idioma, teclado, `PATH`, ativação sudo, criação de
  usuário e adição de grupos adicionais, ativação do NetworkManager se necessário,
  serviço NTP para o fuso-horário, inittab runlevel 4 e configuração do SlackPkg
  e atualização do sistema.
