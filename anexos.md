
# ANEXO 02  
## Documentación técnica resumida


## 1. Arquitectura general

**KONOMEGUA IA** utiliza una arquitectura modular basada en una API backend, procesos asíncronos y servicios auxiliares para procesamiento documental e inteligencia artificial.

```text
[Usuario / Frontend]
        |
        v
[FastAPI Backend]
        |
        v
[PostgreSQL]
        |
        v
[RabbitMQ]
        |
        v
[TDR Worker]
        |
        v
[PDF / OCR / Extracción de texto]
        |
        v
[Chunking + Embeddings]
        |
        v
[ChromaDB]
        |
        v
[vLLM]
        |
        v
[Requerimientos técnicos]
```


## 2. Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Python | Lenguaje principal del backend. |
| FastAPI | Framework para API. |
| Uvicorn | Servidor ASGI. |
| PostgreSQL | Base de datos relacional. |
| RabbitMQ | Cola de mensajes. |
| Playwright | Navegación automatizada y obtención de token. |
| pdfplumber | Extracción de texto desde PDF. |
| Tesseract OCR | Reconocimiento de texto en documentos escaneados. |
| ChromaDB | Base vectorial. |
| LangChain | Orquestación de prompts, modelos y parsers. |
| vLLM | Servidor de inferencia compatible con OpenAI API. |
| Docker | Contenerización y despliegue. |


## 3. Estructura lógica

```text
KONOMEGUA IA
├── app.py
├── scripts/
├── services/
│   ├── ai/
│   ├── queue/
│   ├── rag/
│   └── runpod/
├── workers/
└── tests/
```


## 4. Flujo de procesamiento TDR

1. Se detecta una convocatoria relevante.
2. Se identifican ítems o productos asociados.
3. Se publica un job en RabbitMQ.
4. El worker descarga el PDF oficial.
5. Se extrae texto mediante pdfplumber.
6. Si el PDF es escaneado, se aplica OCR.
7. El texto se divide en fragmentos.
8. Los fragmentos se indexan en ChromaDB.
9. Se recupera contexto relevante para cada producto.
10. El LLM extrae requerimientos técnicos.
11. Los resultados se guardan en base de datos.


## 5. Seguridad técnica

El sistema usa variables de entorno para credenciales y configuración sensible. Los archivos `.env` reales no deben ser incluidos en repositorios ni anexos públicos.

---

# ANEXO 03  
## Manual de instalación y despliegue

## 1. Requisitos generales

Para ejecutar **KONOMEGUA IA** se requiere:

- Python 3.11 o superior.
- Docker y Docker Compose.
- PostgreSQL o conexión a base de datos compatible.
- RabbitMQ.
- ChromaDB.
- Servidor vLLM para inferencia IA.
- Tesseract OCR y Poppler para procesamiento documental.

## 2. Ejecución con Docker Compose

Desde la raíz del proyecto:

```bash
docker compose up --build -d
```

Servicios principales:

| Servicio | Descripción |
|---|---|
| api | Backend FastAPI. |
| worker | Procesador asíncrono de TDR. |
| rabbitmq | Cola de mensajes. |
| chromadb | Base vectorial. |

---

## 3. Ejecución de servidor IA vLLM

En servidor GPU:

```bash
docker compose -f docker-compose.gpu.yml up -d
```

O usando el script:

```bash
bash scripts/start_vllm.sh
```


## 4. Variables de entorno

El sistema usa variables de entorno como:

```env
DB_HOST=
DB_PORT=
DB_NAME=
DB_USER=
DB_PASSWORD=
RABBITMQ_URL=
CHROMA_HOST=
CHROMA_PORT=
VLLM_BASE_URL=
VLLM_MODEL=
RUNPOD_API_KEY=
RUNPOD_POD_ID=
PROXY_URL=
HUGGING_FACE_HUB_TOKEN=
```


## 5. Validación de despliegue

Verificar:

```bash
docker ps
```

Probar API:

```bash
curl http://localhost:8080/api/catalogos/estados
```

Probar vLLM:

```bash
curl http://localhost:8000/v1/models
```

## 6. Observaciones

No se deben incluir claves reales, contraseñas ni tokens en archivos versionados. Usar `.env.example` como referencia.

---

# ANEXO 05  
## Licencias y componentes de terceros

## 1. Finalidad

Este anexo identifica las principales librerías, frameworks y herramientas de terceros utilizadas por **KONOMEGUA IA**.


## 2. Componentes principales

| Componente | Uso | Tipo |
|---|---|---|
| FastAPI | Framework API | Librería Python |
| Uvicorn | Servidor ASGI | Librería Python |
| Playwright | Automatización web | Librería/herramienta |
| Requests | Cliente HTTP | Librería Python |
| python-dotenv | Variables de entorno | Librería Python |
| pdfplumber | Extracción de PDF | Librería Python |
| pdf2image | Conversión PDF a imagen | Librería Python |
| pytesseract | OCR | Librería Python |
| Tesseract OCR | Motor OCR | Herramienta externa |
| Pillow | Procesamiento de imágenes | Librería Python |
| psycopg2-binary | Conexión PostgreSQL | Librería Python |
| aio-pika | RabbitMQ async | Librería Python |
| httpx | Cliente HTTP async | Librería Python |
| LangChain | Orquestación IA | Librería Python |
| ChromaDB | Base vectorial | Librería Python / servicio |
| sentence-transformers | Embeddings | Librería Python |
| vLLM | Servidor IA | Framework IA |
| Docker | Contenedores | Herramienta externa |
| RabbitMQ | Broker de mensajes | Servicio externo |
| PostgreSQL | Base de datos | Servicio externo |


## 3. Recomendación para validación

Para validar licencias exactas según versiones instaladas:

```bash
pip install pip-licenses
pip-licenses --format=markdown > THIRD_PARTY_LICENSES.md
```


## 4. Observación

Las librerías de terceros no forman parte de la autoría original del software. Se utilizan como dependencias para la ejecución, despliegue y operación del sistema.

---

# ANEXO 07  
## Declaración jurada de originalidad

Mediante el presente, el suscrito, la empresa de personería jurídica denominada KONOMEGUA E.I.R.L., con R.U.C. N° 20615360946, con domicilio legal en CALLE LOS PACAES NRO. 214 URB LOS CACTUS, La Molina, Lima, e-mail comercial@konomegua.com, debidamente representada por el señor CARLOS ENRIQUE CATAÑO AYBAR, identificado con D.N.I. N° 44433198, en su calidad de Gerente General de KONOMEGUA E.I.R.L., declaro bajo juramento que el software denominado **ITACHI IA** ha sido desarrollado como una obra original, y que cuento con los derechos correspondientes sobre su código fuente, estructura funcional, documentación y elementos propios desarrollados.

Asimismo, declaro que el software puede utilizar componentes, librerías, frameworks o herramientas de terceros, los cuales se encuentran identificados en el anexo de licencias y componentes externos, sin que ello implique apropiación de derechos de terceros.

El presente documento se emite para fines de sustento documental, registro, revisión o presentación ante la entidad correspondiente.

---

**Lugar:** Lima  
**Fecha:** 26 de mayo del 2026

**Firma:** _______________________________  

**Nombre:** CARLOS ENRIQUE CATAÑO AYBAR      

**DNI / RUC:** 44433198 / 20615360946

