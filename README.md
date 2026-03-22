# Proyecto Celeste

Base del proyecto del curso de mecanica celeste.

## Requisitos

- Python 3.13
- Git
- VS Code (con extensiones Python y Jupyter)

## Puesta en marcha rapida

1. Clonar el repositorio:

```bash
git clone https://github.com/LFCA17/Proyecto_celeste.git
cd Proyecto_celeste
```

2. Crear y activar entorno virtual:

Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Instalar dependencias:

```bash
pip install -r requirements.txt
```

## Uso en VS Code (notebooks)

1. Abrir la carpeta del proyecto en VS Code.
2. Abrir cualquier archivo `.ipynb`.
3. Seleccionar el kernel de Python del entorno `.venv`.
4. Ejecutar celdas normalmente.

## Notas

- Si en Windows falla `rebound`, instala Microsoft C++ Build Tools (MSVC 14+) e intenta de nuevo.
- Para instalar paquetes desde notebook, usar `%pip install paquete` para asegurar instalacion en el kernel activo.
