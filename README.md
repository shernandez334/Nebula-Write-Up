## 🛸 Nebula - Write-Up

### Task 3: ###
---

### 3.1. ¿Qué puerto tiene abierto por UDP? ###

Para determinar el puerto UDP abierto en el sistema objetivo, utilizaremos la herramienta de escaneo de red **Nmap**. Necesitamos escanear específicamente los puertos UDP utilizando la opción `-sU`.

1.  Ejecuta el siguiente comando de Nmap, reemplazando `[IP_del_objetivo]` con la dirección IP real del objetivo:

    ```bash
    nmap -sU [IP_del_objetivo]
    ```

    La opción `-sU` indica a Nmap que realice un escaneo UDP.

2.  La salida del comando mostrará los puertos UDP abiertos, cerrados y filtrados. La siguiente imagen muestra la salida relevante:

    ![Salida del Escaneo UDP de Nmap](https://github.com/user-attachments/assets/70afb17f-506e-4bcf-bc55-89f1e0808baf)

    *Esta imagen muestra los resultados del escaneo UDP de Nmap.*

**✅ RESPUESTA:**

    53
---------------------------------------------------------------------------------------------------------------------------------------------

### 3.3. ¿Qué puerto tiene corriendo el servicio domain? ###

<img width="523" alt="Captura de pantalla 2025-04-16 a las 13 16 32" src="https://github.com/user-attachments/assets/3ab919ae-8f8e-4577-b2b8-b098fb2545c5" />


**✅ RESPUESTA:**

    53

---------------------------------------------------------------------------------------------------------------------------------------------

### 3.4. ¿Cuántos puertos TCP tiene cerrados? ###
Para saber que puertos TCP estan cerrados, debemos escanear todos los puertos tcp que ahi, y, a diferencia del escaneo anterior, debemos escanear puertos TCP, lo que reuiere un comando diferente.

El comando que se usó fue:
  ```bash
  nmap -sT [IP_DEL_OBJETIVO] -p-
  ```

 * `nmap`: Invoca la herramienta de escaneo de red Nmap.
* `-sT`: Realiza un escaneo TCP Connect para verificar el estado de los puertos estableciendo conexiones completas.
* `[IP_DEL_OBJETIVO]`: Especifica la dirección IP del sistema que se va a escanear.
* `-p-`: Indica que se deben escanear todos los 65535 puertos TCP posibles.

<img width="752" alt="Captura de pantalla 2025-04-17 a las 11 54 13" src="https://github.com/user-attachments/assets/1b13b870-025b-4ac0-87fb-33f7bf307899" />

*Este es el resultado del escaneo completo de los puertos TCP.*

**✅ RESPUESTA:**

    65532
---------------------------------------------------------------------------------------------------------------------------------------------

### 3.5 ¿Cuál es el bind.version? ###

Ya tenemos los puertos UPD y TCP abiertos, pero hasta ahora solo hemos hecho un escaneo superficial. Necesitamos excarvas más y para ello necesitamos las versiones, traceroutes, etc. En este caso, paara obtener más informacion sobre las versiones de servicios de los puertos abiertos, utilizó el siguiente comando:
```bash
nmap -A IP
```

* nmap: herramienta de escaneo de red.

* -A: activa múltiples opciones avanzadas:

    * Detección del sistema operativo (-O)

    * Detección de versión de servicios (-sV)

    * Script scanning (--script=default)

    * Traceroute

<img width="711" alt="Captura de pantalla 2025-04-17 a las 12 06 05" src="https://github.com/user-attachments/assets/4bd0f7c8-a241-4c01-8b9b-622d4502fd39" />

*Este es el resultado del comando.*

**📘 ¿Qué es BIND?**

BIND (Berkeley Internet Name Domain) es el servidor DNS más utilizado en internet. Es responsable de traducir nombres de dominio (como google.com) en direcciones IP (como 142.250.186.110).

**✅ RESPUESTA:**

    9.9.5-3ubuntu0.19-Ubuntu

---------------------------------------------------------------------------------------------------------------------------------------------
### 3.6 ¿Cuál es el bind.version? ###

<img width="768" alt="Captura de pantalla 2025-04-17 a las 12 15 21" src="https://github.com/user-attachments/assets/53970afe-2702-44b8-b114-7e55e63ad3e4" />

*Título web.*

**📘 ¿Qué es el título de una pagina web?**

El título de una web es el texto que aparece en la pestaña del navegador cuando visitas una página web. También es el texto principal que aparece como enlace azul en los resultados de búsqueda de Google.

**✅ RESPUESTA:**

    Bluffer V.0.1a
---------------------------------------------------------------------------------------------------------------------------------------------
### 3.7 ¿Por qué puerto corre el servicio licensedaemon? ###

Primero que todo, necesitamos saber qué tipo de protocolo utiliza licensedaemon. Según IBM, este servicio opera mediante el protocolo TCP/IP. Teniendo esto en cuenta, realizaremos un escaneo de puertos TCP. Como los escaneos anteriores no detectaron el servicio, es probable que licensedaemon no esté utilizando un puerto común. Por ello, haremos un escaneo completo de todos los puertos.

El comando que utilizamos fue:
```bash
nmap -sT IP_DEL_OBJETIVO -p-
```

<img width="746" alt="Captura de pantalla 2025-04-17 a las 12 59 27" src="https://github.com/user-attachments/assets/dc54ff5e-8dbb-45d3-9a84-99bfdb2d0755" />

*Resultado del escaneo.*

**📘 ¿Qué es licensedaemon?**

El daemon lmgrd y los daemons proveedor trabajan juntos para gestionar las claves de licencia. El daemon lmgrd gestiona el contacto inicial con los programas de aplicación del cliente, pasando la conexión al daemon proveedor apropiado. El daemon lmgrd también inicia y reinicia los daemons proveedores.

De forma predeterminada, el daemon lmgrd en Windows es un servicio de Windows.

El daemon lmgrd se inicia en el puerto TCP/IP 27000 de forma predeterminada al iniciar el servidor. Si no ha configurado un cortafuegos, lmgrd asigna un número de puerto TCP/IP aleatorio al daemon del proveedor e inicia el daemon del proveedor en dicho puerto.

**✅ RESPUESTA:**

    1986
---------------------------------------------------------------------------------------------------------------------------------------------
### 3.8 ¿Qué versión de SSH tiene Nebula? ###

Hasta este punto, hemos recopilado bastante información valiosa del servidor. Sin embargo, todavía no hemos encontrado acceso al servicio SSH de la máquina Nebula. Esto se debe a que los comandos anteriores no realizaron un escaneo completo de todos los puertos, y por lo tanto no detectaron puertos menos comunes, como el 1986.

Este puerto, que inicialmente parecía estar relacionado únicamente con el servicio licensedaemon, resultó ser clave: allí se aloja una licencia de OpenSSH, y a través de esta se puede acceder a información crucial como la versión del servicio y otros datos relevantes.

El comando es:
```bash
nmap -sVC IP_DEL_OBJETIVO -p-
```

* nmap: Ejecuta la herramienta de escaneo de red.

* -sV: Detecta las versiones de los servicios que se están ejecutando en los puertos abiertos.

* -sC: Ejecuta los scripts por defecto de Nmap (equivale a --script=default). Estos scripts pueden obtener información adicional útil como banners, títulos de páginas web,             usuarios por defecto, etc.

* -p-: Escanea todos los puertos TCP (del 1 al 65535).

<img width="1037" alt="Captura de pantalla 2025-04-17 a las 13 36 07" src="https://github.com/user-attachments/assets/fd4e35c9-9868-4697-94de-9d7a9f3f5243" />

*El equivalente al comando -sVC es -A*

**✅ RESPUESTA:**

    OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
---------------------------------------------------------------------------------------------------------------------------------------------
### 3.9 ¿Cuál es la Public Key de ED25519? ###

<img width="1034" alt="Captura de pantalla 2025-04-17 a las 13 47 39" src="https://github.com/user-attachments/assets/52a0ee7b-d7ce-4d40-9159-65d208244bd1" />


*Respuesta del comando anterior*

**✅ RESPUESTA:**

    b1:7b:06:a9:49:85:1e:2a:0a:de:71:9d:8b:50:d3:4a

### Task 4: ###
---

### 4.1 ¿Qué servicio es vulnerable a ataques de man-in-the-middle? ###

**✅ RESPUESTA:**

    SSH    
---------------------------------------------------------------------------------------------------------------------------------------------
### 4.2 ¿Cuál es el CVSS asociado a la vulnerabilidad? ###

Para identificar con mayor precisión las vulnerabilidades presentes en los puertos abiertos(53, 80, 1986), se emplea la herramienta Nessus. Si bien Nmap dispone de scripts que permiten realizar detección de vulnerabilidades (mediante la opción --script), Nessus ofrece una interfaz más intuitiva y automatizada que facilita el proceso de análisis. Además, proporciona informes detallados que incluyen descripciones, niveles de severidad y recomendaciones para mitigar los hallazgos, lo cual resulta fundamental para tareas de evaluación y documentación de la seguridad de la red. 

Abajo está un tutorial de Nessus y de como encontramos la vulnerabilidad.

<img width="1708" alt="Captura de pantalla 2025-04-17 a las 15 47 27" src="https://github.com/user-attachments/assets/8593b46c-fe8c-49f2-8c52-771f281ab7f3" />

<img width="1710" alt="Captura de pantalla 2025-04-17 a las 15 51 37" src="https://github.com/user-attachments/assets/62f2c6e1-1829-427d-b684-c1893263d6bc" />

<img width="1413" alt="Captura de pantalla 2025-04-17 a las 16 03 21" src="https://github.com/user-attachments/assets/e166244a-feea-4a5b-ad2b-6af0bec00e49" />

<img width="1710" alt="Captura de pantalla 2025-04-17 a las 15 53 17" src="https://github.com/user-attachments/assets/22db78e3-a8d0-44b3-9435-2a3817150fc9" />

<img width="1443" alt="Captura de pantalla 2025-04-17 a las 16 32 40" src="https://github.com/user-attachments/assets/0500ba86-d97a-4ab9-96e5-ecdaafa66555" />

**✅ RESPUESTA:**

    5,9

---------------------------------------------------------------------------------------------------------------------------------------------
### 4.3 ¿Cómo se llama popularmente el ataque? ###

Una vez identificada la vulnerabilidad con Nessus, accedí a la información detallada proporcionada por la herramienta haciendo clic sobre el hallazgo correspondiente.

<img width="1452" alt="Captura de pantalla 2025-04-17 a las 16 39 51" src="https://github.com/user-attachments/assets/b43450f5-7de8-41a6-a505-31867f040527" />

Encontre que el nombre completo de la vulnerabilidad es: SSH Terrapin Prefix Truncation Weakness.

<img width="1439" alt="Captura de pantalla 2025-04-17 a las 16 40 46" src="https://github.com/user-attachments/assets/ff409d9f-8823-4277-8534-8ddbd42d6d37" />

Sin embargo, el nombre proporcionado por Nessus no coincidía con la denominación comúnmente utilizada para esta vulnerabilidad. Por ello, realicé una búsqueda en Google utilizando el nombre completo del hallazgo y encontré información relevante en la siguiente página:


https://www.suse.com/es-es/support/kb/doc/?id=000021295

<img width="1707" alt="Captura de pantalla 2025-04-17 a las 16 41 52" src="https://github.com/user-attachments/assets/8b075283-baa1-4446-bf80-92094b6ac756" />

**✅ RESPUESTA:**

    Terrapin Attack

---------------------------------------------------------------------------------------------------------------------------------------------
### 4.4 ¿Cuál es el CVE asociado a la vulnerabilidad más alta de SSH? ###

<img width="1437" alt="Captura de pantalla 2025-04-17 a las 16 55 29" src="https://github.com/user-attachments/assets/192ca819-0479-4342-88dd-406ed9c0ff34" />

**✅ RESPUESTA:**

    CVE-2023-48795

---------------------------------------------------------------------------------------------------------------------------------------------
### 4.5 ¿Cuál es el CVE asociado a la vulnerabilidad más alta de SSH? ###


<img width="1004" alt="Captura de pantalla 2025-04-17 a las 17 00 14" src="https://github.com/user-attachments/assets/e6e8e142-82a3-459d-a06c-9d0289380964" />

**✅ RESPUESTA:**

    2,1

---------------------------------------------------------------------------------------------------------------------------------------------
### 4.6 ¿Cómo se llama la vulnerabilidad? ###

<img width="1450" alt="Captura de pantalla 2025-04-17 a las 17 07 37" src="https://github.com/user-attachments/assets/84efe136-34c6-43db-b8ef-953ed1e1fc34" />

<img width="1443" alt="Captura de pantalla 2025-04-17 a las 17 08 13" src="https://github.com/user-attachments/assets/cb7f0fd9-f839-457d-a4f9-245767f9334d" />

**✅ RESPUESTA:**

    ICMP Timestamp Request Remote Date Disclosure

---------------------------------------------------------------------------------------------------------------------------------------------
### 4.7 ¿Qué Plugin la ha detectado? ###

<img width="1456" alt="Captura de pantalla 2025-04-17 a las 17 14 02" src="https://github.com/user-attachments/assets/c9ed66d9-fe6e-49aa-8723-165981481f8f" />

**✅ RESPUESTA:**

    10114

### TASK 5: ###
---
**INTRODUCCIÓN A LAS SALA**

Una transferencia de zona DNS, a veces llamada AXFR por el tipo de solicitud, es un tipo de transacción de DNS. Es uno de varios mecanismos disponibles para administradores para replicar bases de datos DNS a través de un conjunto de servidores DNS. La transferencia puede hacerse de dos formas: completa (AXFR) o incremental (IXFR)

### 5.1 ¿Cuál es el código de verificación de google site? ###

En base a la introudcción, podemos inferir que el ejercicio va a tener que ver con la zona de transferencia. 

**🔍 ¿Qué es AXFR?**

AXFR (Asynchronous Full Transfer) es un tipo de consulta DNS que permite transferir toda la zona DNS de un dominio desde un servidor al cliente que lo solicita.

Para acceder a esta, en el caso de que no este bien configurada, usamos el siguiente comando:

```bash
dig axfr nebula.io @IP_DEL_OBJETIVO
```

* dig: herramienta de consultas DNS.

* axfr: tipo de consulta que solicita una transferencia completa de la zona DNS.

* nebula.io: dominio objetivo.

* @IP_DEL_OBJETIVO: IP del servidor DNS autoritativo para el dominio.

<img width="1588" alt="Captura de pantalla 2025-04-17 a las 18 15 35" src="https://github.com/user-attachments/assets/6f104a20-03e7-471c-89e0-c0bd28e9d63b" />

**✅ RESPUESTA:**

    tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA

---------------------------------------------------------------------------------------------------------------------------------------------
### 5.2 ¿A qué IP apunta el dominio de nebula.io? ###

**🌐 ¿Qué es un registro A en DNS?**

Un registro A (Address) es un tipo de registro DNS que asocia un nombre de dominio (como nebula.io) con una dirección IPv4.

<img width="1614" alt="Captura de pantalla 2025-04-17 a las 18 24 02" src="https://github.com/user-attachments/assets/0e8511d0-be57-48a9-97ec-6354ccc43e0c" />

**✅ RESPUESTA:**

    192.168.150.144
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.3 ¿A qué IP apunta el dominio de nebula.io? ###

**🌍 ¿Qué es un registro AAAA?**

Un registro AAAA (cuatro A) en DNS es como el registro A, pero para direcciones IPv6.

<img width="1602" alt="Captura de pantalla 2025-04-17 a las 18 27 20" src="https://github.com/user-attachments/assets/fcd3213b-39a0-4592-920b-403274251b53" />

**✅ RESPUESTA:**

    dead:beef::1
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.4 ¿Cuál es el TTL del ftp? ###

<img width="1590" alt="Captura de pantalla 2025-04-17 a las 18 35 37" src="https://github.com/user-attachments/assets/5c3a93c1-9db4-444c-8f61-0d3cc00c3729" />

**✅ RESPUESTA:**

    7200
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.5 Nebula se conecta a través de VPN por la ip… ###

<img width="1598" alt="Captura de pantalla 2025-04-17 a las 21 25 45" src="https://github.com/user-attachments/assets/db5c5901-59c8-453b-b2fc-26f3909aa951" />

**✅ RESPUESTA:**

    198.51.100.10
---------------------------------------------------------------------------------------------------------------------------------------------  
### 5.6 Nebula se conecta al correo electrónico por la ip… ###

<img width="1710" alt="Captura de pantalla 2025-04-17 a las 21 28 44" src="https://github.com/user-attachments/assets/946ae151-063a-4917-af1a-8d0bc4f40a28" />

**✅ RESPUESTA:**

    192.168.150.146
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.7 En el registro PTR, ¿cuál es el prefijo inverso? ###

<img width="1606" alt="Captura de pantalla 2025-04-17 a las 21 31 57" src="https://github.com/user-attachments/assets/fe34ec51-9de7-4911-bbdf-73acb012b788" />

**✅ RESPUESTA:**

    144.150.168.192.IN-ADDR.ARPA.nebula.io.
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.8 ¿Con quien contactarías en caso de necesitar ayuda con el soporte? ###

<img width="1598" alt="Captura de pantalla 2025-04-17 a las 21 34 23" src="https://github.com/user-attachments/assets/b5328905-83d1-4c25-8188-4af1321ee2c3" />

**✅ RESPUESTA:**

    admin@nebula.io
---------------------------------------------------------------------------------------------------------------------------------------------
### 5.9 ¿Cuál es la información del servidor de nebula.io? ###

<img width="1591" alt="Captura de pantalla 2025-04-17 a las 21 39 00" src="https://github.com/user-attachments/assets/b27c5136-3905-4f13-a6a4-e1810a734f51" />

**✅ RESPUESTA:**

    "Nebula Server" "Linux"

 ---------------------------------------------------------------------------------------------------------------------------------------------
### 5.10 Encuentra la flag ###

<img width="1599" alt="Captura de pantalla 2025-04-17 a las 21 40 50" src="https://github.com/user-attachments/assets/eb0d80db-9c32-463a-958d-b7be5b683218" />

**✅ RESPUESTA:**

    "BLUFFER{S3cr3t_DNS_Tr4nsfer_Flag}"
