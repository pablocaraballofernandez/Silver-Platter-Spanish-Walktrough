<div align="center">
  
# TryHackMe - Silver Platter

</div>

<div align="center">
 
## GUÍA COMPLETA EN ESPAÑOL 


  [![TryHackMe](https://img.shields.io/badge/Platform-TryHackMe-success?style=for-the-badge)](#)
  [![TryHackME](https://img.shields.io/badge/Difficulty-Easy-blue?style=for-the-badge)](#)
  [![TryHackME](https://img.shields.io/badge/OS-Linux-orange?style=for-the-badge)](#)

</div>

# Indice  

## · Reconocimiento  

## · Bypass  

## · Obtención de credenciales

## · Entrada como usuario

## · Escalada de privilegios

## Reconocimiento  

Como siempre digo, como buen hacker que somos, vamos a lanzar un nmap para el escaneo de puertos:

![Imágenes](Images/1.png)

Podemos observar tres puertos abiertos, de los cuales el que mas me llama la atención es el nginx ya que es el alojado en el puerto 80 y la versión es algo antigua, asi que vamos a empezar por ahí.

Observando la página, encontramos lo siguiente en el directorio de contacto:

![Imágenes](Images/2.png)

Tenemos un mensaje que nos dice lo siguiente: 

"Si deseas ponerte en contacto con nosotros, comunícate con nuestro jefe de proyecto en Silverpeas. Su nombre de usuario es "scr1ptkiddy"."

Este mensaje nos revela un nombre de usuario y una aplicación; investigando sobre esta aplicación he conseguido saber que se utiliza principalmente en entornos corporativos o institucionales para mejorar la comunicación interna, la organización del trabajo y la gestión del conocimiento.
Por ejemplo, empresas, universidades, administraciones públicas o asociaciones pueden usarlo como una intranet colaborativa.

Ahora vamos a investigar el servicio del puerto 8080 del cual no obtenemos respuesta, pero, si buscamos un directorio en ese puerto llamado como la aplicación que mencionamos antes, encontramos el siguiente login:

![Imágenes](Images/3.png)

**¿Cómo he encontrado este login?**

Debido a que en mi pequeña investigación sobre la app de silverpeas, descubrí que la aplicación tiene interfaz web con lo que probé a buscar el directorio en el nginx pero no hubo respuesta, al contrario que en el puerto 8080.  

## Bypass

Ahora buscaremos en internet algún CVE de esta app, y encontraremos la siguiente página:  

![Imágenes](Images/4.png)

Aqui nos comenta de un posible bypass eliminando la password en la petición post de login, pero nos hace falta un username, buscando un poco más encontré el siguiente repo:  

![Imágenes](Images/5.png)  

Ahora abriremos la burpsuite e interceptaremos la petición de login:  

![Imágenes](Images/6.png)  

Modificamos la petición eliminando la password:

![Imágenes](Images/7.png)  

Y enviamos la petición:  

![Imágenes](Images/8.png)  

## Obtención de credenciales

Investigando la página, encontré una sección de notificaciones donde encontré unas credenciales para conectarse por ssh:  

![Imágenes](Images/9.png)  

## Entrada como usuario  

Nos conectaremos por ssh usando esas credenciales:  

![Imágenes](Images/10.png)  

![Imágenes](Images/11.png)  

## Escalada de privilegios  

Al hacer cat a /etc/passwd vemos que hay un usuario llamado tyler que posee privilegios de root:  

![Imágenes](Images/12.png)

También se puede observar que nuestro usuario tim pertenece al grupo adm, esto nos permite leer los logs del sistema:  

![Imágenes](Images/13.png)  

**Explicación de comando utilizado**

**cat /var/log/auth***: Muestra el contenido de todos los archivos cuyo nombre empiece por auth en la carpeta /var/log/.

**grep -i pass:** Busca todas las líneas que contengan la palabra “pass”, sin importar mayúsculas o minúsculas (-i = case-insensitive), es decir, busca coincidencias como password, Passwd, failed password, etc.

Ahora cambiamos al usuario tyler usando la contraseña encontrada:  

![Imágenes](Images/14.png)  

Accedemos a root y hacemos cat a root.txt:  

![Imágenes](Images/15.png)  

# DISCLAIMER

Este writeup es SOLO para propósitos educativos.  
Úsalo responsablemente en entornos autorizados como TryHackMe.  

**Autor:** pablocaraballofernandez  
**Plataforma:** TryHackMe

</div>


<div align="center">
  
  [![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/pablo-caraballo-fern%C3%A1ndez-a12938358/)
  [![TryHackMe](https://img.shields.io/badge/TryHackMe-212C42?style=for-the-badge&logo=tryhackme&logoColor=white)](https://tryhackme.com/p/CyberPablo)
  [![HackTheBox](https://img.shields.io/badge/HackTheBox-111927?style=for-the-badge&logo=hackthebox&logoColor=9FEF00)](https://ctf.hackthebox.com/user/profile/872564)
  .[![Cyberdefenders](https://img.shields.io/badge/CyberDefenders-1E3A5F?style=for-the-badge&logo=shield&logoColor=white)](https://cyberdefenders.org/p/cybersecpcarfer)
  
</div>




