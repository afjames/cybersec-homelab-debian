# 🛡️ Homelab de Cibersegurança & Administração Linux

Repositório dedicado à documentação técnica de construção, endurecimento (*hardening*) e acesso remoto multi-estação de um servidor Debian 12 em ambiente headless (*CLI*).

## 🧰 Arquitetura & Especificações do Ambiente

* **Servidor Alvo (Guest):** Debian 12 Bookworm (Sem interface gráfica)
* **Hospedeiro Principal (Host/Gateway):** Ubuntu 24.04 LTS | Asus X555UB
* **Hipervisor:** Oracle VM VirtualBox 7.1
* **Estação Externa de Acesso:** Windows 11 Desktop (`DESKTOP-K99RLH6`)
* **Rede & Mapeamento:** 
  * Modo NAT no VirtualBox com *Port Forwarding* (`Host :2222` -> `Guest :22`).
  * Acesso local via Loopback (`127.0.0.1:2222`).
  * Acesso via Rede Local LAN (`192.168.1.x:2222`).

---

## 📐 Topologia de Acesso Remoto

```text
[ Windows Desktop ] ──(LAN / PowerShell)──┐
                                          ├──> [ Ubuntu Host (UFW :2222) ] ──(VirtualBox NAT)──> [ Debian Server (:22) ]
[ Ubuntu Local ] ────(Loopback / Terminal)─┘
```

## 🚀 Etapas do Projeto

1. Infraestrutura & Virtualização

    [x] Instalação do VirtualBox 7.1 no Ubuntu.

    [x] Assinatura de módulos do Kernel com MOK (Machine-Owner Key) para Secure Boot.

    [x] Instalação e provisionamento do Debian 12 via ISO Netinst em modo texto.

2. Rede Local & Acesso Multi-estação

    [x] Configuração de Redirecionamento de Portas no VirtualBox (2222 -> 22).

    [x] Liberação de tráfego de entrada no firewall do hospedeiro (sudo ufw allow 2222/tcp).

    [x] Estabelecimento de ponte de acesso para a LAN (acesso a partir do Windows via PowerShell).

3. Hardening & Gestão de Identidades (IAM)

    [x] Atualização e manutenção do sistema (apt update && apt upgrade).

    [x] Sincronização de relógio via NTP (systemd-timesyncd).

    [x] Configuração do Firewall Unificado (UFW) no servidor limitando portas ativas.

    [x] Gerenciamento de múltiplas chaves públicas criptográficas (ED25519) no arquivo ~/.ssh/authorized_keys (Chave Ubuntu + Chave Windows).

    [x] Desativação total de login do root e autenticação por senha no SSH (sshd_config).

    [x] Instalação e parametrização do Fail2ban (3 tentativas, janela de 10 min, banimento de 1 hora).

    Bash

# Acesso a partir do próprio Hospedeiro Ubuntu (Loopback)
ssh jamenson@127.0.0.1 -p 2222

# Acesso a partir da Estação Windows (PowerShell / CMD na LAN)
ssh jamenson@192.168.1.105 -p 2222

# Verificação de status do Fail2ban no Debian
sudo fail2ban-client status sshd

