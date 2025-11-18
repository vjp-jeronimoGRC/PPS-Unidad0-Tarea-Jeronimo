## Creación de carpetas

```
mkdir calculator
mkdir docs
mkdir .github
cd .github
mkdir workflows
```

## Creacion de archivos

```
cd .github/workflows
touch CreacionDocumentacion.yml

# Nos movemos a su carpeta padre

nano mkdocs.yml
# Contenido del fichero

site_name: Calculadora Python
nav:
  - Home: index.md
docs_dir: docs
# Guardamos y salimos con Ctrl+O y Ctrl+X

nano requeriments.txt
#Contenido del fichero

tk
mkdocs
# Guardamos y salimos con Ctrl+O y Ctrl+X

```

Despues cambiaremos a escribir los ficheros de las carpetas `docs/` y `calculator/`


## Carpeta calculator/
```
cd /calculator

nano __init__.py
# Contenido del fichero:
from .gui import Calculator
# Ctrl+O Ctrl+X

nano gui.py
# Contenido del fichero:
import tkinter as tk

class Calculator:
    def __init__(self, master):
        self.master = master
        master.title("Calculadora")

        self.result = tk.Entry(master, width=16, font=('Arial', 24), borderwidth=2, relief="ridge")
        self.result.grid(row=0, column=0, columnspan=4)

        buttons = [
            ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
            ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('+', 4, 2), ('=', 4, 3),
        ]

        for (text, row, col) in buttons:
            tk.Button(master, text=text, width=4, height=2, font=('Arial', 18),
                      command=lambda t=text: self.on_click(t)).grid(row=row, column=col)

    def on_click(self, char):
        if char == '=':
            try:
                self.result.delete(0, tk.END)
                self.result.insert(tk.END, str(eval(self.result.get())))
            except Exception:
                self.result.delete(0, tk.END)
                self.result.insert(tk.END, "Error")
        else:
            self.result.insert(tk.END, char)

if __name__ == "__main__":
    root = tk.Tk()
    calc = Calculator(root)
    root.mainloop()

# Ctrl+O Ctrl+X

```
## Carpeta docs/

```
cd /docs

touch index.md
touch git.md
touch gitActions.md
touch gitPages.md
touch docker.md
touch conclusiones.md

# Lo hacemos con el comando touch porque iremos rellenando los archivos conforme avancemos 

```
## Creacion del mkdocs.yml
nano mkdocs.yml
# Contenido

```
site_name: "PPS - Unidad 0 - Tarea de TU_NOMBRE"

theme:
  name: material

nav:
  - Inicio: index.md
  - Git: git.md
  - GitHub Actions: gitActions.md
  - GitHub Pages: gitPages.md
  - Docker + NGINX: docker.md
  - Conclusiones: conclusiones.md

```
# Creación del Workflow de Github Actions

Dentro de .github/workflows/CreacionDocumentacion.yml ponemos lo siguiente:

```
name: Generar Documentación MkDocs

on:
  push:
    branches: [ "main" ]

Robs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install MkDocs
        run: |
          pip install mkdocs mkdocs-material

      - name: Build Docs
        run: mkdocs build --strict

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```
Después por línea de comando ejecutamos lo siguiente:

```
pip install mkdocs mkdocs-material

mkdocs build --strict

mkdocs serve

```
Como resultado podremos visualizar lo siguiente:

![imagen mkdocs](images/Captura_de_pantalla_2025-11-13_231351.png)


## gh-pages

En Github nos vamos a Settings > Pages y seleccionamos Source: GitHub Actions

## Workflow
- Archivo: `.github/workflows/CreacionDocumentacion.yml`
- Configuración principal:
  - Se ejecuta en **push** a `main`.
  - Instala Python 3.10 y MkDocs con el tema Material.
  - Construye la documentación (`mkdocs build`).
  - Publica los archivos generados en la rama `gh-pages` usando `peaceiris/actions-gh-pages@v3`.

```yaml
name: Generar Documentación MkDocs

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - run: pip install mkdocs mkdocs-material
      - run: mkdocs build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```
## Verificación

Ruta: `https://vjp-jeronimogrc.github.io/PPS-Unidad0-Tarea-Jeronimo/`


# Contenedor Docker NGINX 

## Objetivo
Levantar un contenedor NGINX para servir la documentación generada por MkDocs desde la rama `gh-pages`. Aunque la web ya está publicada en GitHub Pages, este ejercicio permite practicar Docker y volúmenes bind mount.

## Comando utilizado

```bash
docker run -d \
  --name PPSUnidad0-Tarea_Jeronimo \
  -p 8085:80 \
  -v $(pwd):/usr/share/nginx/html:ro \
  nginx
```
Tambien para poder ver información del contenedor introducimos el siguiente comando: `docker inspect PPSUnidad0-Tarea_Jeronimo`

