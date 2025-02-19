Crear un programa en Node.js que utilice un paquete de npm sencillo para realizar las siguientes tareas:

1. Instalación de paquete:

    - Utilizando el administrador de paquetes npm, instalar el paquete chalk. Este paquete permite cambiar el color del texto en la consola.

2. Interacción con el usuario:

    - El programa debe pedir al usuario que ingrese su nombre y su edad a través de la consola (utilizando la librería readline de Node.js para interactuar con el usuario).

3. Mostrar mensaje personalizado:
    - Utilizando el paquete chalk, el programa debe mostrar un mensaje de bienvenida que incluya el nombre y la edad del usuario, y que el mensaje se muestre en un color diferente dependiendo de la edad del usuario:
        - Si la edad es menor de 18 años, el mensaje debe aparecer en color rojo.
        - Si la edad está entre 18 y 65 años, el mensaje debe aparecer en color verde.
        - Si la edad es mayor de 65 años, el mensaje debe aparecer en color azul.

---

Modulos utilizados:

-   chalk
-   readline
