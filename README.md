**1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?**

Porque el frontend únicamente se encarga de mostrar la interfaz al usuario, las consultas y modificaicones de datos las realiza el backend, que actúa como intermediario entre el frontend y PostgreSQL

**2. ¿Por qué el backend pertenece a dos redes?**

Porque necesita comunicarse con dos servicios diferentes: con el frontend para recibir las peticiones de la aplicación y también con PostgreSQL para consultar y guardar los datos. Por eso está conectado a las redes frontend y datos

**3. ¿Qué servicio necesita publicar un puerto en el anfitrión?**

El frontend, porque necesitamos acceder a la aplicación desde el navegador de nuestro ordenador
Publicamos el puerto 8080 del ordenador para acceder a Ngnix desde http://localhost:8080

**4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?**

Porque PostgreSQL y el backend debe comunicarse mediante la red interna de Docker. Publicar el puerto de PostgreSQL hacia el exterior no soluciona un problema de configuración de redes y, además, puede suponer un riesgo de seguridad al exponer la base de datos innecesariamente

**5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?**

Docker Compose permite definir y administrar todos los servicios desde un único archivo compose.yaml. Así podemos arrancar, detener y recrear toda la aplicación con pocos comandos, manteniendo configuradas las redes, los volúmenes y las dependencias entre servicios sin tener que ejecutar muchos comandos docker run manualmente

**6. ¿Por qué hemos dividido la arquitectura en dos redes?**

Para mejorar la seguridad y el aislamiento de los servicios. La red frontend permite la comunicación entre el frontend y el backend, mientras que la red datos permite la comunicación entre el backend y PostgreSQL. De esta manera, el frontend no tiene acceso directo a la base de datos

**7. ¿Por qué el backend utiliza `db` como dirección de PostgreSQL?**

Porque Docker Compose proporciona resolución DNS interna entre los servicios de una misma red. El nombre db corresponde al servicio de PostgreSQL y permite que el backend lo encuentre sin necesitar conocer su dirección IP. Utilizar localhost sería incorrecto porque haría referencia al propio contenedor del backend

**8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?**

Al detener el proyecto con docker compose stop, los contenedores dejan de ejecutarse, pero se mantienen y los datos siguen guardados
Al ejecutar docker compose down -v, se eliminan los contenedores, las redes y también los volúmenes, asociados. Esto puede provocar la pérdida de los datos almacenados en PostgreSQL

**9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?**

La empresa perdería todos los datos almacenados en la base de datos, incluyendo las incidencias registradas y su información asociada. Esto podría provocar una pérdida importante de información y afectar al trabajo de los empleados. Por eso es fundamental utilizar volúmenes persistentes y realizar copias de seguridad periódicas
