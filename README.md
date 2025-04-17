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
