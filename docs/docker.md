# Contenedor NGINX para la documentación de la tarea

## Objetivo
Levantar un contenedor NGINX para servir la documentación generada por MkDocs desde la rama `gh-pages`.

## Comando utilizado
```bash
docker run -d \
  --name PPSUnidad0-Tarea_Jeronimo \
  -p 8085:80 \
  -v $(pwd):/usr/share/nginx/html:ro \
  nginx

```
## Ruta de conexión del despliegue de Docker

```
http://localhost:8085
```

[![despliegue_docker](docker.png)

[![docker inspect](inspect.png)

