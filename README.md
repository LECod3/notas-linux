# Guía de Comandos y Configuraciones

Este repositorio contiene notas personales sobre:
- Linux Ubuntu

# pwd (Print Working Directory)

# cd (Change Directory)
"." (directorio actual): Este es el directorio en el que te encuentras actualmente.
".." (directorio superior): Te lleva al directorio por encima del directorio actual.
"~" (directorio personal): Este directorio apunta por defecto al “directorio personal” del usuario.
"-" (directorio anterior): Te lleva al directorio en el que estabas justo antes.

# ls (List Directories)
Ten en cuenta que no todos los archivos en un directorio serán visibles. Los nombres de archivo que empiezan con "." están ocultos. Sin embargo, podés verlos con el comando "ls" usando la opción "-a" (a de "all", todos). "ls -a"

También existe otra opción útil de ls: "-l" (long). Muestra una lista detallada de archivos en formato largo. Te mostrará información detallada, empezando por la izquierda: permisos del archivo, número de enlaces, nombre del propietario, grupo del propietario, tamaño del archivo, fecha y hora de la última modificación, y nombre del archivo/directorio. "ls -l"
drwxr-x--- 7 pete penguingroup   4096 Nov 20 16:37 Desktop

Los comandos tienen cosas llamadas flags (o argumentos u opciones, como prefieras) para agregar más funcionalidad. Fijate que usamos "-a" y "-l"; bueno, podés combinarlas con "-la". El orden de las flags determina en qué orden se aplican. La mayoría de las veces eso no importa, por lo que también podés escribir "ls -al" y seguirá funcionando. "ls -la"

# touch 

# file

# cat

# less

# history

# clear

# cp (Copy)

# mv (Move)

# mkdir (Make Directory)
También podés crear subdirectorios al mismo tiempo con la opción "-p" (parent).
"mkdir -p libros/hemmingway/favoritos"

# rm (Remove)
La opción "-f" o force le indica a "rm" que elimine todos los archivos, estén o no protegidos contra escritura, sin pedir confirmación al usuario (siempre que tengas los permisos adecuados).

# find

# help

# man (Manual)

# whatis

# alias

# tldr

# exit & logout

