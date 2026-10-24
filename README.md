# Hola Marvin - Django

Proyecto sencillo en Django con un servicio que responde un saludo en JSON.

## Como ejecutarlo (Windows CMD)

```cmd
cd C:\Laboratorio
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python manage.py runserver
```

Luego abrir en el navegador: http://127.0.0.1:8000/hola/

Respuesta:

```json
{"mensaje": "Hola Marvin"}
```

## Probar en Postman

1. Abrir Postman y dar click en **Import**.
2. Seleccionar el archivo `hola_marvin.postman_collection.json`.
3. Abrir la peticion **Hola Marvin** (GET `http://127.0.0.1:8000/hola/`).
4. Dar click en **Send** (el servidor tiene que estar corriendo).
