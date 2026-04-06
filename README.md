# DockerLabs Writeups 🐳
 
Writeups de las máquinas que voy resolviendo en **[DockerLabs](https://dockerlabs.es)**, plataforma gratuita de hacking ético creada por El Pingüino de Mario.
 
Cada writeup documenta el proceso completo: reconocimiento, enumeración, explotación, escalada de privilegios y lecciones aprendidas.  
El objetivo no es solo llegar a root — es entender el *por qué* de cada paso.
 
---
 
## 📊 Progreso
 
| Dificultad | Resueltas |
|------------|-----------|
| 🟢 Muy fácil | 3 |
| 🟡 Fácil | 0 |
| 🟠 Media | 0 |
| 🔴 Difícil | 0 |
| **Total** | **3** |
 
---
 
## 📁 Índice de máquinas
 
### 🟢 Muy fácil
 
| Máquina | Técnicas | Writeup |
|---------|----------|---------|
| Firsthacking | FTP · CVE-2011-2523 · Backdoor vsftpd 2.3.4 · netcat | [writeup](./muy-facil/firsthacking/writeup.md) |
| BreakMySSH | Fuerza bruta SSH · Hydra · Metasploit ssh_login | [writeup](./muy-facil/breakmyssh/writeup.md) |
| Trust | Enumeración web · Gobuster · Fuerza bruta SSH · sudo misconfiguration · GTFOBins (vim) | [writeup](./muy-facil/trust/writeup.md) |
 
### 🟡 Fácil
 
> *Pendiente*
 
### 🟠 Media
 
> *Pendiente*
 
### 🔴 Difícil
 
> *Pendiente*
 
---
 
## 📝 Plantilla de writeup
 
Todos los writeups siguen esta estructura:
 
```
## Reconocimiento
## Enumeración  
## Explotación
## Escalada de privilegios
## Lecciones aprendidas
```
 
---
 
## 🛠️ Herramientas habituales
 
`nmap` `gobuster` `hydra` `netcat` `Metasploit` `Burp Suite`  
`find` `sudo -l` `linpeas` `GTFOBins` `searchsploit`
 
---
 
## 📚 Recursos útiles
 
- [DockerLabs](https://dockerlabs.es) — plataforma
- [GTFOBins](https://gtfobins.github.io) — escalada de privilegios Linux
- [HackTricks](https://book.hacktricks.xyz) — referencia general
- [RevShells](https://www.revshells.com) — generador de reverse shells
- [ExploitDB](https://www.exploit-db.com) — base de datos de exploits
 
