# 📘 Documentação Técnica do Projeto Home Lab

## 1. Visão Geral

O projeto **Home Lab** tem como objetivo principal proporcionar um ambiente controlado e seguro para estudo, testes e implementação de tecnologias voltadas para infraestrutura de redes, servidores e serviços de nuvem privada. É mantido por um estudante de Ciências da Computação com foco em aprendizado prático e profissionalização do ambiente doméstico de rede.

---

## 2. Infraestrutura Física

### 2.1 Link de Internet
- Provedor local com autenticação PPPoE  
- Conversor de fibra → MikroTik RB450G

### 2.2 Gateway Principal
- **Equipamento**: MikroTik RB450G  
- **Função**: Roteador principal, gerenciando PPPoE, NAT, VLANs e DHCP  

**Portas ocupadas:**
- Porta 1: Link PPPoE (conversor de fibra)  
- Porta 2: Access Point 1 (Wireless)  
- Porta 3: Access Point 2 (Wireless)  
- Porta 4: Dell Optiplex com Proxmox VE  
- Porta 5: PC principal do usuário

### 2.3 Servidor Principal
- **Modelo**: Dell Optiplex 3070  
- **Especificações**: Intel Core i5, 16GB RAM, 256GB SSD + 1TB HDD SATA  
- **Sistema**: Proxmox VE (hipervisor de virtualização)

---

## 3. Infraestrutura Lógica

### 3.1 Segmentação de Rede (Personalizada)

Bloco: `10.100.0.0/16` (máscara `255.255.0.0`)  
Segmentos usados e planejados:

| Faixa IP         | Função / Grupo                                      |
|------------------|-----------------------------------------------------|
| 10.100.0.x       | Equipamentos e serviços centrais (RB, VPN, Zabbix) |
| 10.100.1.x       | VMs e Containers (Proxmox, Ubuntu Server, etc)     |
| 10.100.3.x       | Dispositivos de expansão (APs, switches)           |
| 10.100.6.x       | Dispositivos IoT ou baixa prioridade (planejado)   |
| 10.100.9.x       | Equipamentos críticos ou com acesso remoto         |
| 10.100.10.x      | Rede principal (dispositivos confiáveis)           |
| 10.100.20.x      | Rede de servidores e serviços internos             |
| 10.100.30.x      | Rede para convidados (com isolamento)              |

> 💡 Convenção: terminações em 3, 6 e 9 para identificar ativos fixos e críticos.

### 3.2 VLANs Planejadas

| VLAN ID | Nome      | Função                                  | Faixa IP          |
|---------|-----------|-----------------------------------------|-------------------|
| 10      | Principal | Dispositivos confiáveis e gerenciáveis  | 10.100.10.0/24    |
| 20      | Servidores| Contêineres, VMs e serviços internos    | 10.100.20.0/24    |
| 30      | Convidados| Wi-Fi para visitantes, acesso isolado   | 10.100.30.0/24    |

### 3.3 Proxmox VE

Contêineres e VMs ativos:

| Tipo      | Nome           | Função                                         |
|-----------|----------------|-----------------------------------------------|
| Container | minecraft      | Servidor de jogos para amigos e família       |
| Container | zabbix-server  | Monitoramento de rede e serviços              |
| VM        | tailscale      | VPN e acesso remoto seguro                    |
| VM        | casaos         | Orquestração de containers com UI web         |
| Container | whatsapp-bot   | Bot assistente via WhatsApp (planejado)       |
| VM/CT     | nextcloud      | Nuvem privada para arquivos e backups         |
| Container | pi-hole        | Bloqueio de anúncios na rede                  |

---

## 4. Domínio Local (Planejado)

### 4.1 Controlador de Domínio
- **Solução**: Samba 4 AD DC (ou Windows Server)  
- **Hostname**: `dc1.homelab.nzs`  
- **Domínio**: `homelab.nzs`

### 4.2 Funções
- Gerenciamento centralizado de usuários  
- DNS interno  
- Integração com compartilhamentos de rede e Nextcloud  
- GPOs para mapeamento de unidades e políticas de segurança  

---

## 5. Segurança

### 5.1 M
