# Slackware Scripts 🐧

Este repositório é o meu canivete suíço pessoal para o Slackware.
Uma coleção direto ao ponto de scripts para automatizar qualquer tarefa,
configuração ou personalização do sistema.

## 🌐 Como configurar a Rede e baixar os scripts no instalador (BusyBox)

Como o ambiente de boot do Slackware não possui Git instalado nativamente,
você deve ativar a rede no terminal e usar o `wget` do BusyBox para baixar os
scripts brutos (raw).

### 1. Ativar a Rede no Instalador

**Para ver o nome da sua Internet:**
```bash
ip link
```

**Para ativar a conexão via dhcp (exemplo eth0):**
```bash
dhcpcd eth0
```

**Testar a conexão:**
```bash
ping -c4 8.8.8.8
```

### 2. Como Baixar e Executar qualquer Script

Para baixar qualquer script deste repositório usando o `wget` do BusyBox,
use a URL do arquivo no modo **Raw**.

**Estrutura do comando:**
```bash
wget https://raw.githubusercontent.com/LinuxProativo/SlackScripts/master/<SCRIPT>
```

**Exemplo prático de execução:**
```bash
wget https://raw.githubusercontent.com/LinuxProativo/SlackScripts/master/btrfs-uefi
chmod +x btrfs-uefi
./btrfs-uefi
```

## 📝 Lista de Scripts Disponíveis

*   `btrfs-uefi` - Automatiza o particionamento em NVMe, GPT, UEFI, formatação em Btrfs com subvolumes `@` e `@home`, além de gerar o `fstab` otimizado.
