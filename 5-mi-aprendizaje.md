# Mi aprendizaje - Práctica 2

### Reflexión Profesional: Antes y Después de la Práctica
Al iniciar esta práctica, mi conocimiento de Docker se centraba en despliegues básicos. Tras completar los ejercicios, he logrado pasar de una "dockerización superficial" a una gestión de infraestructura mucho más profesional.

Como **junior developer**, la interacción con pgAdmin y PostgreSQL fue trabajo conocido; es una herramienta que manejo diariamente en mi rol de desarrollo Full Stack. Sin embargo, el valor agregado no estuvo en el manejo de la base de datos como tal, sino en aprender a orquestar el entorno: aislar el servidor, gestionar su red y configurar su acceso mediante variables de entorno en lugar de configuraciones manuales en el host.

### Principales Aprendizajes Técnicos

* **Variables de Entorno y Seguridad:** Entiendo ahora que las variables de entorno son el estándar para inyectar configuraciones dinámicas. Para el proyecto que estoy dockerizando actualmente en mi trabajo, esto es vital para desacoplar las credenciales del código fuente y permitir despliegues consistentes en diferentes entornos.
* **Arquitecturas de Red (Networking):** La capacidad de crear redes tipo `bridge` personalizadas (`net-curso`, `net-wp`) me permite diseñar arquitecturas de microservicios aisladas. El ejercicio de conectar un contenedor a dos redes simultáneamente fue clave para visualizar cómo funcionan los "puentes" de comunicación en infraestructuras complejas.
* **Persistencia y Efimeralidad:** La prueba con WordPress y MySQL fue reveladora. Aunque considero que WordPress es una tecnología más lenta y descentralizada comparada con el desarrollo actual asistido por IA, el ejercicio sirvió para validar que la capa de aplicación debe ser efímera. Podemos destruir y recrear el contenedor de la App sin temor, siempre que la persistencia resida en el contenedor de datos.

### Resolución de Problemas (Troubleshooting)
Durante la práctica en entorno Windows (usando Git Bash), documenté dos soluciones críticas:

1.  **Traducción de rutas:** El error al intentar acceder a rutas de Linux desde Git Bash (el problema de `C:/Program Files/Git/...`). Lo solucioné anteponiendo la doble barra `//` al comando, forzando a la terminal a enviar la ruta literal al contenedor.
2.  **Conflictos de TTY:** Identifiqué el error de los saltos de línea (`\r`) al usar el modo interactivo `-i` sin asignar una terminal `-t`, lo cual es un aprendizaje fundamental para depurar contenedores en entornos híbridos.

### Investigación: Gestión de datos confidenciales con Docker Secrets
Para gestionar información altamente sensible (como llaves privadas SSL o contraseñas de producción), las variables de entorno pueden no ser suficientes ya que son visibles mediante un `docker inspect`.

**Docker Secrets** es la solución profesional para esto:

* **Funcionamiento:** Los secretos se cifran durante el tránsito y mientras están en reposo dentro del clúster (Docker Swarm).
* **Acceso Seguro:** Solo los servicios que han recibido acceso explícito pueden ver el secreto. Este se monta en un sistema de archivos temporal (`/run/secrets/<secret_name>`) dentro del contenedor, lo que significa que nunca se escribe en el disco duro del contenedor ni se expone en los logs.
* **Ventaja:** Permite una gestión centralizada y segura, ideal para arquitecturas donde la seguridad es la máxima prioridad.