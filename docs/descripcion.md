# Descripción del Proyecto: Detección de Enfermedades en Gallinas Usando IA y Concurrencia

Este proyecto tiene como objetivo desarrollar una aplicación para la detección automática de enfermedades en gallinas mediante imágenes, utilizando técnicas de inteligencia artificial (IA) y procesamiento distribuido. Se emplean herramientas avanzadas de concurrencia y comunicación entre procesos para asegurar que el sistema sea eficiente, escalable y no bloquee el flujo de trabajo. A continuación, se detallan los componentes del proyecto y cómo se implementan los distintos requisitos técnicos solicitados por el profesor.

# Objetivos Principales:
1. Captura de imágenes de gallinas: Los usuarios, a través de una app Android, pueden tomar imágenes de las gallinas y enviarlas al servidor para su análisis.
2. Análisis con IA (TensorFlow): El servidor procesa las imágenes utilizando un modelo de IA basado en TensorFlow, que detecta posibles signos de enfermedad en las gallinas.
3. Procesamiento concurrente y distribuido: Para garantizar eficiencia y escalabilidad, se utiliza Celery con Redis para distribuir las tareas entre múltiples procesos de worker, procesando varias imágenes simultáneamente.
4. Notificaciones en tiempo real: A través de WebSockets (SocketIO), se notifica al cliente Android cuando los resultados del análisis están listos, permitiendo un flujo interactivo entre cliente y servidor.
5. Persistencia de datos: Los resultados del análisis, junto con las imágenes procesadas, se almacenan en una base de datos PostgreSQL para su posterior consulta.


