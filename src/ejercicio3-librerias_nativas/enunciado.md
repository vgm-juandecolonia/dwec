Crea un programa en Node.js que realice las siguientes tareas utilizando librerías nativas os y fs:

1.  Información del Sistema:

    -   Utilizando la librería os, el programa debe mostrar información del sistema operativo, como:
        -   Nombre del sistema operativo.
        -   Versión del sistema operativo.
        -   Cantidad de memoria disponible.
        -   Número de CPUs disponibles.

2.  Crear y Escribir en un Archivo:

    -   Utilizando la librería fs, el programa debe crear un archivo de texto llamado info_sistema.txt en el que se guardará la información del sistema recolectada en el primer paso.
    -   El archivo debe contener las siguientes líneas:
        -   El nombre del sistema operativo.
        -   La versión del sistema operativo.
        -   La cantidad de memoria disponible (en MB).
        -   El número de CPUs disponibles.

3.  Leer y Mostrar el Contenido del Archivo:

    -   Después de haber creado el archivo info_sistema.txt, el programa debe leerlo y mostrar su contenido por consola.

Requisitos:

-   El programa debe ser ejecutado desde la línea de comandos.
-   La información debe ser guardada en un archivo .txt dentro del mismo directorio donde se ejecuta el programa.
-   El programa debe manejar cualquier error potencial, como problemas al escribir o leer el archivo.

Restricciones:

-   Solo se pueden utilizar las librerías nativas de Node.js como os y fs.
-   El archivo debe ser creado y gestionado de manera síncrona o asíncrona según se prefiera, pero debe cumplir con los requisitos de la tarea.

---

Módulos utilizados:

-   os
-   fs
