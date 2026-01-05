# Ejemplo de como implementar DOCKER + NodeRED + Dashboard + LOGO8 de SIEMENS

## 1_Para instalar Docker Compose en Windows, sigue estos pasos:

* Abre tu navegador web y visita el sitio web oficial de Docker ( https://www.docker.com/products/docker-desktop/ )

* Descarga el instalador de Docker Desktop para Windows y ejecútalo... Sigue las instrucciones del instalador para completar la instalación de Docker Desktop en tu máquina.

---

## 2_Una vez que la instalación haya finalizado, abre una ventana de línea de comandos (Command Prompt) o PowerShell, y ejecuta el siguiente comando para verificar que Docker Compose se haya instalado correctamente:

```cpp
docker-compose --versión$$
```

Si el comando muestra la versión de Docker Compose instalada, Docker Compose está listo para su uso en tu sistema Windows.

<img width="1365" height="717" alt="DockerDesktop" src="https://github.com/user-attachments/assets/80098b9f-95ca-4c1d-a457-1dfdd98eaf7e" />

---

## 3_ Descargar la imagen de Node-RED:
Si bien internamente desde docker en la seccion de paleta ("Manage palette") o simplemente en la casilla de buscador central podemos buscar las librerias/dependencias que requerimos, también podemos abrir el terminal y descarga la imagen oficial más reciente para asegurar compatibilidad con las funciones

```cpp
docker pull nodered/node-red$$
```

<img width="1365" height="717" alt="docker_NodeRED" src="https://github.com/user-attachments/assets/f9a1c049-5e12-4a77-865e-465cf792c38d" />

---

## 4_Crear un Volumen para Persistencia:
Para evitar perder tus flujos y configuraciones, o sea, tu trabajo / proyecto si el contenedor se borra, crea un volumen de datos:

```cpp
docker volume create node_red_data$$
```
---

## 5_Ejecutar el Contenedor:
Usa el siguiente comando para iniciar Node-RED. Este mapea el puerto 1880 y conecta el volumen de datos

```cpp
docker run -d -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red$$
```

**Donde:**
* **-d:** Ejecuta el contenedor en segundo plano (modo detached).
* **-p 1880:1880:** Mapea el puerto del contenedor al de tu máquina.
* **-v node_red_data:/data:** Persiste tus datos en el volumen creado.
* **--name mynodered:** Asigna un nombre amigable al contenedor.

---

## 6_Acceder a Node-RED
Una vez ejecutado, abre tu navegador y entra en la siguiente dirección: http://localhost:1880

o en mi caso particular: http://127.0.0.1:1880/#flow/f6f2187d.f17ca8

<img width="1365" height="713" alt="docker_NodeRED_RUNproyecto" src="https://github.com/user-attachments/assets/e9279731-c986-423d-b1f4-6f68528bdb36" />

Siempre verifiquen que al ejecutar les indicará la dirección 

---
## El 1er EJEMPLO:

<img width="1365" height="718" alt="ProyectoLOGO_NodeRED" src="https://github.com/user-attachments/assets/e529a6ba-1987-4e01-907f-9ac8edda6ccc" />

En el ejemplo que comparto, si bien hay muchas formas de poder tener control de entradas y salidas de nuestro LOGO8 de SIEMENS desde nuestro dashboard con Docker y NodeRED; Yo elegí la mas sencilla, que es escribir y leer los espacios de memoria "VM" (byte 0 hasta 849 y bit 0 hasta el 7)…
Instaladas las dependencias, tal como lo muestro en las imágenes, nuestro siguiente paso es implementar los nodos funcionales "S7" que son:

* **S7 in**  (Lee datos que provienen de un PLC)
* **S7 out** (Enviamos datos y sobrescribimos espacios de memorias determinados en un PLC)

Como no queremos interactuar directamente con los espacios de memoria que afectan a la salida directamente, vamos a implementar los registros VM del LOGO escribiendo DB1,0.0 para el pulsador "ON"("DB1", "Byte 0 al 849" . "Bit del 0 al 7"), y seguiremos el orden consecutivamente:
OFF DB1,0.1
PE  DB1,0.2
IZQ DB1,0.3
DER DB1,0.4
KM1 DB1,0.5
KM2 DB1,0.6
KM3 DB1,0.7

<img width="1365" height="719" alt="variables_1" src="https://github.com/user-attachments/assets/1864c9f4-e37e-43dc-a353-85181b4cf6b5" />

Recuerden que la configuración del byte y bit de cada nodo, tenemos que reproducirlo en nuestra función "Entrada de RED" y "Salida de RED" en nuestro programa del LOGO8 SIEMENS
En la próxima edición, vamos hablar de como generar nuestros DASHBOARD cono Docker y NodeRED...

<img width="1359" height="718" alt="LOGO_NodeRED_1" src="https://github.com/user-attachments/assets/799b6160-4987-4ef4-819b-0e44939f1500" />

<img width="1365" height="720" alt="LOGO_NodeRED_2" src="https://github.com/user-attachments/assets/9ac0fdf7-0e03-4f8d-8b23-432ad189aee8" />

<img width="1365" height="717" alt="LOGO_NodeRED_3" src="https://github.com/user-attachments/assets/8b263294-5458-44f7-88fa-edec4026b8d8" />

<img width="1365" height="716" alt="LOGO_NodeRED_4" src="https://github.com/user-attachments/assets/ae6cdefe-d838-4289-af0d-d714aa50757a" />

**Las dependencias que necesitamos son:**

<img width="705" height="580" alt="dependencias_2" src="https://github.com/user-attachments/assets/bb982207-7a9b-45db-b584-46eba4f37d78" />

Para instalarlas, nos dirigimos a la seccion "Administrar paleta" y en la barra de busqueda vamos a buscar las librerias que a continuacion les muestro:

<img width="1365" height="717" alt="dependencias_1" src="https://github.com/user-attachments/assets/e5eaef1d-2f41-499b-997a-41e871938379" />

<img width="706" height="584" alt="dependencias_3" src="https://github.com/user-attachments/assets/4eb8cdba-1e37-4346-87ef-dd132a8d8734" />


## Referencias:

https://www.freecodecamp.org/news/the-docker-handbook/

https://www.thesmarthomebook.com/2021/04/14/basics-connecting-home-assistant-to-node-red/

https://www.youtube.com/watch?v=wi2b5ZcySuc

https://www.youtube.com/watch?v=sIvzWJ4ytok

https://www.youtube.com/watch?v=0c9TbB2IzAs

https://www.youtube.com/watch?v=8-o8Oo7CsMM

https://www.prometec.net/jugando-con-nodered-y-dashboard/ + https://www.youtube.com/watch?v=l3CTWBKJWR0





