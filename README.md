# Hola Marvin - Django (Arquitectura Hexagonal)

Proyecto sencillo en Django con un servicio que responde un saludo en JSON,
organizado con **arquitectura hexagonal** (puertos y adaptadores).

## Esquema de la arquitectura

```
   Postman / navegador
           |
           v
+--------------------------------------------------+
|  INFRASTRUCTURE (adaptador de entrada)           |
|  infrastructure/web/urls.py  -> rutas            |
|  infrastructure/web/views.py -> vistas Django    |
+--------------------------------------------------+
           |  llama a ejecutar(nombre)
           v
+--------------------------------------------------+
|  APPLICATION                                     |
|  ports/saludar_use_case.py  -> SaludarUseCase    |
|            ^ implementa                          |
|  services/saludar_service.py -> SaludarService   |
+--------------------------------------------------+
           |  crea Persona y llama saludar()
           v
+--------------------------------------------------+
|  DOMAIN                                          |
|  persona.py -> Persona("Marvin").saludar()       |
+--------------------------------------------------+
```

Las dependencias van **de afuera hacia adentro**: la infraestructura conoce a la
aplicacion, la aplicacion conoce al dominio, y el dominio no conoce a nadie.

```
saludo/
├── application/
│   ├── ports/
│   │   └── saludar_use_case.py   # puerto de entrada (clase abstracta)
│   └── services/
│       └── saludar_service.py    # implementa el caso de uso
├── domain/
│   └── persona.py                # clase Persona (regla del negocio)
└── infrastructure/
    └── web/
        ├── urls.py               # rutas
        └── views.py              # adaptador de entrada (Django)
```

- **Domain**: `Persona` arma el texto "Hola <nombre>". No sabe nada de Django.
- **Ports**: `SaludarUseCase` es el contrato (`ejecutar(nombre)`) que dice que sabe hacer la app.
- **Services**: `SaludarService` implementa ese contrato usando `Persona`.
- **Infrastructure**: `views.py` traduce HTTP/JSON a una llamada al caso de uso y la respuesta a JSON.

## Como ejecutarlo (Windows CMD)

```cmd
cd C:\Laboratorio
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python manage.py runserver
```

## Endpoints

| Metodo | URL | Body | Respuesta |
|--------|-----|------|-----------|
| GET | `/hola/<nombre>/` | - | `{"mensaje": "Hola <nombre>"}` |
| POST | `/hola/` | `{"nombre": "Marvin"}` | `{"mensaje": "Hola Marvin"}` |

Si en el POST falta `nombre` o el JSON es invalido, responde **400** con `{"error": "..."}`.

## Probar en Postman

1. Abrir Postman y dar click en **Import**.
2. Seleccionar el archivo `hola_marvin.postman_collection.json`.
3. El servidor tiene que estar corriendo.

**Hola por ruta**

- GET `http://127.0.0.1:8000/hola/Marvin/`
- Respuesta: `{"mensaje": "Hola Marvin"}`

**Hola por JSON**

- POST `http://127.0.0.1:8000/hola/`
- Header: `Content-Type: application/json`
- Body -> raw -> JSON:

```json
{"nombre": "Marvin"}
```

- Respuesta: `{"mensaje": "Hola Marvin"}`

**Ejemplo de error** (body `{}`):

```json
{"error": "Falta el campo nombre"}
```
