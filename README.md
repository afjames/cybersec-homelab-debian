# 🛡️ Homelab de Cibersegurança & Administração Linux

Repositório dedicado à documentação técnica da construção e endurecimento (*hardening*) de um servidor Debian 12 em ambiente headless (*CLI*).

## 🧰 Especificações do Ambiente
* **Hospedeiro (Host):** Ubuntu 24.04 LTS | Asus X555UB
* **Hipervisor:** Oracle VM VirtualBox 7.1
* **Sistema Convidado (Guest):** Debian 12 (Sem interface gráfica)
* **Rede:** Modo NAT com Port Forwarding (`Host 127.0.0.1:2222` -> `Guest :22`)

## 🚀 Etapas do Projeto

### 1. Infraestrutura & Virtualização
- [x] Instalação do VirtualBox 7.1 no Ubuntu Noble.
- [x] Configuração e assinatura de módulos do Kernel com MOK (Machine-Owner Key) / Secure Boot.
- [x] Instalação do Debian 12 via ISO Netinst em modo texto.

### 2. Acesso Remoto & Redirecionamento
- [x] Mapeamento de porta de loopback (`127.0.0.1:2222`) para acesso SSH isolado.
- [x] Adição do usuário comum ao grupo de administradores (`sudoers`).

### 3. Hardening & Segurança Inicial
- [x] Atualização de pacotes do sistema (`apt update && apt upgrade`).
- [x] Sincronização e ajuste de relógio via NTP (`systemd-timesyncd`).
- [x] Configuração e ativação do Firewall Unificado (`UFW`) limitando acesso à porta 22/tcp.
- [x] Implementação de Autenticação por Chaves Criptográficas (`ED25519`).
- [ ] Desativação de login root e autenticação por senha no SSH (`sshd_config`).
- [ ] Instalação e parametrização do `Fail2ban` contra ataques de força bruta.

## 🛠️ Comandos Principais Utilizados
```bash
# Acesso SSH via porta alta do hospedeiro
ssh jamenson@127.0.0.1 -p 2222

# Verificação do status do Firewall
sudo ufw status verbose