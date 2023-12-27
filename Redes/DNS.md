DNS es un sistema que se utiliza para traducir los nombres de dominio legibles por humanos, como www.ejemplo.com, en direcciones IP (Protocolo de Internet) numéricas. 

-------------------------

Podemos realizar también consultas DNS para obtener información sobre nombres de dominio o para traducir un nombre de dominio en una dirección IP correspondiente de las siguientes formas:
## NSLOOKUP
```bash
nslookup www.google.es
```
![[Pasted image 20230917125148.png]]
- La dirección IP de www.google.es es 142.250.184.3
- El servidor DNS donde hicimos la consulta es 192.168.0.1

## CONSULTA DE NSLOOKUP A SERVIDOR EXTERNO
Podemos también realizar una consulta DNS a un servidor externo en lugar de a nuestro servidor DNS local:
```bash
nslookup www.google.es 1.1.1.1
```
![[Pasted image 20230917125405.png]]

Otros servidores externos útiles pueden ser:
1.1.1.1 Cloudflare
1.0.0.1 Cloudflare
8.8.8.8 Google
8.8.4.4 Google
9.9.9.9 Quad9
9.9.9.10 Quad9
209.244.0.3 Level3
209.244.0.4 Level3
208.67.222.222 OpenDNS
208.67.220.220 OpenDNS