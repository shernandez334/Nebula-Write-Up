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

**✅ Respuesta:**

**PUERTO 53**

---------------------------------------------------------------------------------------------------------------------------------------------

### 3.3. ¿Qué puerto tiene corriendo el servicio domain? ###

<img width="523" alt="Captura de pantalla 2025-04-16 a las 13 16 32" src="https://github.com/user-attachments/assets/3ab919ae-8f8e-4577-b2b8-b098fb2545c5" />

**✅ Respuesta:**



**PUERTO 53**

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

<img width="726" alt="Captura de pantalla 2025-04-16 a las 13 30 13" src="https://github.com/user-attachments/assets/fb32b123-3d14-4a9d-9c6b-f19f57a8c4db" />

**✅ Respuesta:**

**65532**
