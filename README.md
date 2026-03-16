# Xray-Core-Ubuntu-Server---Kali-
Instalação do Xray-core com REALITY (2026)


Servidor Ubuntu

# Atualizar sistema
```
sudo apt update && sudo apt upgrade -y
```
# Instalar Xray-core
```
curl -Ls https://raw.githubusercontent.com/XTLS/Xray-install/main/install-release.sh | sudo bash
```
# Gerar UUID e chave privada
```
xray uuid
xray x25519
```
Configurar domínio:

Crie ghost.bunqrlabs.com apontando para o IP do servidor. 
Editar configuração:
```
sudo nano /usr/local/etc/xray/config.json
```
```
{
  "inbounds": [{
    "listen": "0.0.0.0",
    "port": 443,
    "protocol": "vless",
    "settings": {
      "clients": [
        { "id": "SEU_ID", "flow": "xtls-rprx-vision" }
      ],
      "decryption": "none"
    },
    "streamSettings": {
      "network": "tcp",
      "security": "reality",
      "realitySettings": {
        "show": false,
        "dest": "www.microsoft.com:443",
        "serverNames": ["www.microsoft.com"],
        "privateKey": "REALITY_PRIVATE_KEY",
        "shortIds": ["80", "81"]
      }
    }
  }],
  "outbounds": [{
    "protocol": "freedom"
  }],
  "dns": {
    "servers": [
      "8.8.8.8",
      "1.1.1.1"
    ]
  },
  "routing": {
    "rules": [
      {
        "type": "field",
        "port": 53,
        "network": "udp",
        "outboundTag": "freedom"
      }
    ]
  }
}
```
Habilitar e iniciar:
```
sudo systemctl enable xray
sudo systemctl start xray
sudo ufw allow 443/tcp
```
Cliente Kali Linux
# Instalar snapd
```
sudo apt install snapd -y
sudo systemctl enable --now snapd apparmor
```
# Instalar v2rayA
```
sudo snap install v2raya
```
# Conectar permissões
```
sudo snap connect v2raya:firewall-control
sudo snap connect v2raya:network-control
sudo snap start v2raya
```
Acessar interface: http://127.0.0.1:2017

# Configurar servidor:
```
Protocol: vless
Address: dominio.com
Port: 443
ID: c35cec65-5a66-4c17-8fe6-29296037669d
Flow: xtls-rprx-vision
Security: reality
Dest: www.microsoft.com:443
Server Name: www.microsoft.com
Private Key: SUA_PRIVATE_KEY
Short ID: 80
```
# Configurar proxy:
```
Settings → Transparent Proxy → On
Forward DNS Requests: on
Implementation: TProxy
Traffic Splitting Mode: Proxy All Traffic 
Salvar → Selecionar servidor → Start
```
✅ Teste em https://dnsleaktest.com
