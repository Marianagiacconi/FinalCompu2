# Funcionalidades de Cada Entidad

# Cliente Android
- Captura de Imágenes: El usuario puede capturar imágenes de las gallinas con la cámara del dispositivo móvil.
- Envía Imágenes: Una vez capturada la imagen, la aplicación envía esta imagen al servidor Flask mediante una conexión HTTP utilizando sockets.
- Recepción de Notificaciones: La aplicación escucha notificaciones del servidor en tiempo real mediante WebSockets (SocketIO), lo que permite recibir el diagnóstico sin tener que hacer consultas constantes.
- Muestra Estado de Salud: La aplicación muestra el estado de salud de la gallina (sana o enferma) según el análisis recibido.

# Requisitos Técnicos:
- Sockets (Socket.IO): Se usa para recibir las notificaciones en tiempo real.
- I/O Asíncrona: El envío y recepción de las imágenes y notificaciones ocurre de manera no bloqueante.



# Servidor Flask
- Recepción de Imágenes: El servidor recibe las imágenes enviadas por el cliente Android y las guarda temporalmente en el servidor.
- Creación de Tareas Asíncronas: Una vez recibida la imagen, se crea una tarea para procesarla utilizando Celery y Redis. Esta tarea es gestionada de manera asíncrona para no bloquear el servidor.
- Procesamiento con TensorFlow: La tarea de Celery invoca TensorFlow para procesar la imagen y determinar el estado de salud de la gallina. Esto simula un análisis avanzado de IA.
- Almacenamiento de Resultados: Los resultados del análisis se almacenan en una base de datos PostgreSQL junto con la imagen para referencia futura.
- Notificaciones en Tiempo Real: Una vez completado el análisis, se notifica al cliente Android mediante WebSockets, permitiendo recibir el diagnóstico en tiempo real.

# Requisitos Técnicos:
- Sockets (TCP): Usamos sockets para la recepción de imágenes.
- Asincronismo de I/O: Flask procesa la recepción de imágenes de manera no bloqueante y lanza tareas Celery de forma asíncrona.
- Colas de Tareas Distribuidas (Celery + Redis)**: Celery distribuye el procesamiento de imágenes entre varios workers utilizando Redis como broker de mensajes.
- IPC (Inter-Process Communication): Celery utiliza Redis para manejar la comunicación entre los distintos procesos de worker y el servidor Flask.
- Almacenamiento en PostgreSQL: Los resultados de los análisis y las imágenes procesadas se almacenan de forma segura en una base de datos PostgreSQL.

---

# Celery Workers
- Procesamiento de Imágenes: Celery se encarga de procesar las imágenes en segundo plano. Se utiliza un modelo de TensorFlow para analizar las imágenes y determinar si la gallina está sana o enferma.
- Procesamiento Asíncrono: Gracias a Celery, el procesamiento se distribuye de manera eficiente entre varios workers, lo que permite manejar múltiples tareas simultáneamente sin bloquear el servidor.
- Distribución de Tareas: Cada worker recibe una imagen y la procesa de manera independiente, asegurando que el sistema sea escalable.
- Resultados del Análisis: Una vez completado el análisis, los workers envían el resultado de vuelta al servidor para su almacenamiento y notificación al cliente.

# Requisitos Técnicos:
- TensorFlow: Se utiliza para el análisis de las imágenes, implementando una red neuronal que detecta signos de enfermedades.
- Procesamiento Distribuido: Celery distribuye las tareas de procesamiento entre los workers.
- Colas de Tareas: Redis gestiona las colas de tareas, asegurando que cada imagen sea procesada en paralelo.
- Broker de Mensajes (Redis): Redis actúa como intermediario para la comunicación entre el servidor y los workers de Celery.



# Redis
- Broker de Mensajes: Redis facilita la comunicación entre el servidor Flask y los workers de Celery. Cada vez que una imagen es recibida, Redis se encarga de gestionar la cola de tareas y asegurar que cada worker reciba una tarea.
- Manejo de Tareas Distribuidas: Redis gestiona eficientemente el envío de tareas a los workers y permite una escalabilidad horizontal para manejar múltiples imágenes de manera concurrente.



# PostgreSQL
- Almacenamiento de Imágenes: Las imágenes enviadas por los usuarios son almacenadas en PostgreSQL, lo que permite tener un registro histórico de cada imagen procesada.
- Almacenamiento de Resultados: Los resultados del análisis de las imágenes también se almacenan en la base de datos, permitiendo la trazabilidad y consultas posteriores sobre la salud de las gallinas.
- Consultas de Historial: PostgreSQL permite realizar consultas para obtener el historial de análisis de salud de las gallinas, proporcionando una referencia para estudios de salud a largo plazo.



# WebSockets (SocketIO)
- Notificaciones en Tiempo Real: Cuando el análisis de una imagen está completo, el servidor utiliza WebSockets para notificar al cliente Android en tiempo real. Esto elimina la necesidad de realizar peticiones repetitivas al servidor (polling), lo que mejora la eficiencia del sistema.
- Comunicación Bidireccional: Los WebSockets permiten una comunicación bidireccional entre el servidor y el cliente Android, asegurando que los resultados lleguen de forma inmediata.



# Requisitos Adicionales Cumplidos:
- Parseo de Argumentos por Línea de Comandos: El servidor Flask puede ser iniciado con parámetros desde la línea de comandos que configuran detalles como el puerto del servidor o la ubicación de la base de datos, cumpliendo así con los requisitos de parseo de argumentos.
- Docker: El sistema sera desplegado utilizando Docker, lo que facilita la portabilidad y la instalación de dependencias, asegurando que todo el entorno se configure correctamente en cualquier máquina.

