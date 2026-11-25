1. ¿Por qué Pedidos no puede usar localhost:8081 dentro de su contenedor? ¿Qué lo reemplazó?

Porque dentro de Docker cada contenedor tiene su propio entorno de red. Por lo tanto, cuando Pedidos utiliza localhost, intenta conectarse a sí mismo y no al contenedor de Catálogo.

Se reemplazó localhost:8081 por el nombre del servicio de Docker Compose, por ejemplo, http://catalogo:8081. Docker utiliza su DNS interno para permitir que los contenedores se comuniquen entre sí.



2. ¿Qué ventaja tiene haber puesto la dirección de Catálogo en application.properties y no escrita en el código Java?

La principal ventaja es que permite modificar la dirección del servicio sin tener que cambiar el código Java ni volver a compilar la aplicación. Esto facilita la configuración para diferentes entornos, como desarrollo, pruebas y producción, además de mantener el código más flexible y fácil de mantener.


3. ¿Por qué el Dockerfile tiene dos etapas? ¿Qué pasaría con el tamaño de la imagen si fuera una sola?

El Dockerfile utiliza dos etapas mediante Multi-stage Build. La primera etapa se encarga de compilar el proyecto y generar el archivo JAR utilizando las herramientas necesarias. La segunda etapa copia únicamente ese archivo y lo ejecuta en una imagen más ligera.

Si se utilizara una sola etapa, la imagen final tendría un tamaño mayor porque incluiría herramientas de compilación, dependencias y archivos que no son necesarios para ejecutar la aplicación.


4. ¿Qué pasó con el producto que crearon después de reiniciar Catálogo? ¿Por qué? ¿Qué se necesitaría para que no se pierda?

El producto que se creó desapareció después de reiniciar el contenedor porque los datos estaban almacenados dentro del sistema de archivos del contenedor y no en un almacenamiento persistente.

Para evitar que esto suceda, se necesita configurar un volumen de Docker que permita guardar los datos fuera del ciclo de vida del contenedor. Así, aunque Catálogo se detenga, se reinicie o se vuelva a crear, la información seguirá disponible.


5. depends_on arranca Catálogo antes que Pedidos. ¿Garantiza eso que Catálogo ya esté listo para responder cuando Pedidos recibe su primer pedido?

No. depends_on garantiza el orden de inicio de los contenedores, pero no que la aplicación Catálogo ya esté completamente iniciada y lista para recibir solicitudes.

Para solucionar esto, se pueden implementar healthchecks en Docker Compose y utilizar condition: service_healthy para que Pedidos espere hasta que Catálogo supere su comprobación de salud antes de iniciar. También es recomendable implementar mecanismos de reintento en Pedidos por si Catálogo tarda en responder o deja de estar disponible temporalmente.