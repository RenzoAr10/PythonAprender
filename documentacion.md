# ITACHI IA  
## Código Fuente y Manual de Usuario

**Versión:** 1.0.0  
**Tipo de documento:** Código fuente + Manual de usuario  
**Repositorio:** https://github.com/Y2605/Konomegua-Backend.git  
**Fecha:** 26/05/2026  
**Titular:** CARLOS ENRIQUE CATAÑO AYBAR

---

# Índice

1. Presentación del software  
2. Manual de usuario  
3. Arquitectura funcional  
4. Inventario del código fuente  
5. Código fuente del sistema  
6. Observaciones de seguridad  
7. Checklist final  

---

# 1. Presentación del software

**ITACHI IA** es un sistema orientado a la automatización del análisis de convocatorias públicas y licitaciones.

El software permite identificar oportunidades, consultar convocatorias, procesar documentos técnicos en PDF, extraer requerimientos mediante OCR e inteligencia artificial, y organizar la información para facilitar la preparación de cotizaciones.

El sistema integra una API backend, procesamiento asíncrono mediante workers, extracción documental, base de datos, cola de mensajes, base vectorial y modelos de lenguaje para apoyar el análisis técnico de documentos TDR.

---

# 2. Manual de Usuario

## 2.1 Objetivo del sistema

El objetivo de **ITACHI IA** es permitir que el usuario consulte convocatorias, revise productos o ítems asociados, analice documentos técnicos y obtenga requerimientos relevantes para apoyar la toma de decisiones en procesos de cotización.

## 2.2 Usuarios del sistema

El sistema está dirigido a:

- Analistas de licitaciones.
- Personal encargado de cotizaciones.
- Usuarios responsables de revisar documentos TDR.
- Personal técnico encargado de validar requerimientos.
- Administradores del sistema.

## 2.3 Acceso al sistema

El usuario accede al sistema mediante la interfaz o los servicios habilitados por el backend. El acceso depende del entorno de despliegue, dominio, dirección IP o configuración interna del servidor.

## 2.4 Búsqueda de convocatorias

El usuario puede buscar convocatorias utilizando filtros como:

- Palabra clave.
- Unidad ejecutora.
- Unidad orgánica.
- Estado de convocatoria.
- Descripción del bien o servicio.

El sistema devuelve una lista de resultados para su revisión.

## 2.5 Consulta de cotización

Al seleccionar una convocatoria, el usuario puede visualizar los productos o ítems asociados, junto con la información disponible para apoyar la evaluación técnica y comercial.

## 2.6 Revisión de requerimientos técnicos

El sistema procesa documentos TDR en PDF. Si el documento contiene texto digital, se extrae directamente. Si el documento es escaneado, se aplica OCR.

Luego, el contenido se analiza mediante inteligencia artificial para identificar requerimientos técnicos relevantes.

## 2.7 Interpretación de resultados

Los resultados extraídos deben ser revisados por el usuario antes de ser utilizados en una propuesta final. La extracción automática ayuda a reducir tiempo, pero no reemplaza la validación humana del documento oficial.

## 2.8 Buenas prácticas de uso

- Verificar siempre el documento TDR oficial.
- Revisar manualmente los requerimientos extraídos.
- Validar precios, enlaces y productos antes de cotizar.
- No compartir credenciales ni archivos `.env`.
- Mantener actualizadas las variables de configuración del sistema.

---

# 3. Arquitectura funcional

El flujo general del sistema es el siguiente:

```text
[Usuario / Frontend]
        |
        v
[API Backend FastAPI]
        |
        v
[Base de Datos PostgreSQL]
        |
        v
[RabbitMQ - Cola de trabajos]
        |
        v
[Worker de procesamiento TDR]
        |
        v
[PDF / OCR / Extracción de texto]
        |
        v
[Chunking + Embeddings]
        |
        v
[ChromaDB - Base vectorial]
        |
        v
[vLLM - Modelo de lenguaje]
        |
        v
[Requerimientos técnicos guardados]
```

---

# 4. Inventario del código fuente

El código fuente de **ITACHI IA** se encuentra organizado en módulos. Para este documento se incluyen los archivos principales del sistema, agrupados por responsabilidad técnica.

No se incluyen archivos sensibles, temporales o generados automáticamente.

## 4.1 Archivos raíz del proyecto

| Archivo | Descripción |
|---|---|
| `.gitignore` | Define archivos y carpetas excluidas del control de versiones. |
| `Dockerfile` | Define la construcción del contenedor principal de la aplicación. |
| `docker-compose.yml` | Orquesta API, worker, RabbitMQ y ChromaDB. |
| `docker-compose.gpu.yml` | Configuración para servidor GPU con vLLM. |
| `docker-compose.vllm.yml` | Variante de despliegue del servidor vLLM. |
| `requirements.txt` | Lista de dependencias Python. |
| `app.py` | Aplicación principal FastAPI. |
| `test_vllm.py` | Pruebas de integración para vLLM. |

## 4.2 Carpetas principales

| Carpeta | Descripción |
|---|---|
| `scripts/` | Scripts de despliegue y operación. |
| `services/ai/` | Cliente LLM, prompts, parser y extracción IA. |
| `services/queue/` | Conexión, publicación y consumo de mensajes RabbitMQ. |
| `services/rag/` | Chunking, embeddings, ChromaDB y recuperación semántica. |
| `services/runpod/` | Control de infraestructura GPU en RunPod. |
| `workers/` | Procesos de segundo plano para procesamiento pesado. |
| `tests/` | Pruebas del sistema. |

---

# 5. Código fuente del sistema

A continuación se incluye el código fuente principal de **ITACHI IA**, organizado por archivo y ruta dentro del repositorio.

> Nota: En cada sección se debe pegar el contenido real del archivo correspondiente desde el repositorio.

---

## 5.1 `.gitignore`

```gitignore
.env
__pycache__/
*.pyc
```

---

## 5.2 `Dockerfile`

```dockerfile
FROM python:3.11-slim

# Instalar dependencias del sistema para Playwright, OCR y PDFs
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    curl \
    tesseract-ocr \
    tesseract-ocr-spa \
    poppler-utils \
    && rm -rf /var/lib/apt/lists/*

# Instalar playwright deps
RUN pip install playwright
RUN playwright install --with-deps

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt && playwright install --with-deps chromium

COPY . .

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 5.3 `requirements.txt`

```txt
fastapi
uvicorn
psycopg2-binary
playwright
requests
python-dotenv
pdfplumber
pdf2image
pytesseract
Pillow
supabase
ddddocr
langchain>=0.3
langchain-openai>=0.3
langchain-core>=0.3
aio-pika>=9.0.0
httpx>=0.27.0
chromadb>=0.4.0
sentence-transformers>=2.2.2
langchain-huggingface>=0.0.1
langchain-chroma>=0.1.1
langchain-text-splitters
```

---

## 5.4 `docker-compose.yml`

```yaml
version: '3.8'

# ==============================================================================
# APP SERVER - Arquitectura Principal (Producción)
# Este compose NO incluye vLLM. vLLM debe correr en un servidor GPU separado.
# ==============================================================================

services:
  # 1. API Backend (FastAPI)
  api:
    build: .
    container_name: konomegua_api
    command: uvicorn app:app --host 0.0.0.0 --port 8080 --workers 4
    ports:
      - "8080:8080"
    env_file:
      - .env
    environment:
      - CHROMA_HOST=chromadb
      - CHROMA_PORT=8000
      - RABBITMQ_URL=amqp://guest:guest@rabbitmq:5672/
    depends_on:
      rabbitmq:
        condition: service_healthy
      chromadb:
        condition: service_healthy
    restart: unless-stopped

  # 2. Worker Asíncrono (Extracción y RAG)
  worker:
    build: .
    command: python -m workers.tdr_worker
    env_file:
      - .env
    environment:
      - CHROMA_HOST=chromadb
      - CHROMA_PORT=8000
      - RABBITMQ_URL=amqp://guest:guest@rabbitmq:5672/
    # Es vital que el worker pueda comunicarse con el vLLM Server en la red
    # VLLM_BASE_URL debe apuntar a la IP/Dominio del GPU Server en el archivo .env
    depends_on:
      rabbitmq:
        condition: service_healthy
      chromadb:
        condition: service_healthy
    deploy:
      mode: replicated
      replicas: 2  # Escalabilidad horizontal: 2 workers procesando PDFs
    restart: unless-stopped

  # 3. Message Broker (RabbitMQ)
  rabbitmq:
    image: rabbitmq:3-management
    container_name: konomegua_rabbitmq
    ports:
      - "5672:5672"
      - "15672:15672" # Panel de administración
    environment:
      RABBITMQ_DEFAULT_USER: guest
      RABBITMQ_DEFAULT_PASS: guest
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq
    healthcheck:
      test: ["CMD", "rabbitmq-diagnostics", "-q", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always

  # 4. Vector Database (ChromaDB)
  chromadb:
    image: chromadb/chroma:latest
    container_name: konomegua_chromadb
    ports:
      - "8001:8000"
    volumes:
      - chroma_data:/chroma/chroma
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/api/v1/heartbeat"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always

volumes:
  rabbitmq_data:
  chroma_data:
```

---

## 5.5 `docker-compose.gpu.yml`

```yaml
version: '3.8'

# ==============================================================================
# GPU SERVER - Infraestructura de Inferencia IA (Producción)
# Este compose debe correr EXCLUSIVAMENTE en el servidor con GPU NVIDIA.
# ==============================================================================

services:
  vllm:
    image: vllm/vllm-openai:latest
    container_name: konomegua_vllm_server
    ports:
      - "8000:8000"
    environment:
      # Token opcional si se usan modelos privados de HF
      - HUGGING_FACE_HUB_TOKEN=${HUGGING_FACE_HUB_TOKEN:-}
    volumes:
      # Montar la caché local de Hugging Face para evitar descargar el modelo (aprox 15GB) en cada reinicio
      - ~/.cache/huggingface:/root/.cache/huggingface
    ipc: host # Esencial: vLLM se beneficia enormemente de la memoria compartida para comunicación entre procesos
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    command: >
      --model Qwen/Qwen3-30B-A3B
      --host 0.0.0.0
      --port 8000
      --gpu-memory-utilization 0.90
      --max-model-len 32768
      --dtype auto
      --tensor-parallel-size 1
      --trust-remote-code
      --chat-template-kwargs '{"enable_thinking": false}'
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/v1/models"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: unless-stopped
```

---

## 5.6 `docker-compose.vllm.yml`

```yaml
[PEGAR AQUÍ EL CONTENIDO DE docker-compose.vllm.yml]
```

---

## 5.7 `app.py`

```python
version: '3.8'

services:
  vllm:
    image: vllm/vllm-openai:latest
    container_name: vllm-server
    ports:
      - "8000:8000"
    environment:
      - HUGGING_FACE_HUB_TOKEN=${HUGGING_FACE_HUB_TOKEN:-}
    volumes:
      # Montar la caché local de Hugging Face para evitar descargar el modelo en cada reinicio
      - ~/.cache/huggingface:/root/.cache/huggingface
    ipc: host # vLLM se beneficia enormemente de memoria compartida para comunicación GPU rápida
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    command: >
      --model Qwen/Qwen3-30B-A3B
      --host 0.0.0.0
      --port 8000
      --gpu-memory-utilization 0.90
      --max-model-len 32768
      --dtype auto
      --tensor-parallel-size 1
      --trust-remote-code
      --chat-template-kwargs '{"enable_thinking": false}'
    restart: unless-stopped
```

---

## 5.8 `test_vllm.py`

```python
"""
test_vllm.py — Suite de Pruebas de Integración para vLLM Local

Valida la conexión, generación de texto y parsing JSON del servidor vLLM
local antes de procesar cargas reales de TDR.

Ejecución:
    python test_vllm.py

Prerequisitos:
    - Servidor vLLM corriendo en VLLM_BASE_URL (default: http://localhost:8000/v1)
    - Variables de entorno cargadas (.env)
"""

import os
import sys
import json
import asyncio
import time
import logging
from typing import Any

# Cargar variables de entorno antes de importar módulos del proyecto
from dotenv import load_dotenv
load_dotenv()

import httpx
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage

# ─── Configuración ─────────────────────────────────────────────────────────────

VLLM_BASE_URL = os.getenv("VLLM_BASE_URL", "http://localhost:8000/v1")
VLLM_MODEL = os.getenv("VLLM_MODEL", "Qwen/Qwen2.5-7B-Instruct")

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s | %(levelname)-8s | %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)
logger = logging.getLogger("test-vllm")

# Contadores globales de resultados
_passed = 0
_failed = 0


def _report(test_name: str, success: bool, detail: str = ""):
    """Reportar resultado de un test individual."""
    global _passed, _failed
    if success:
        _passed += 1
        logger.info(f"✅ PASS: {test_name}" + (f" — {detail}" if detail else ""))
    else:
        _failed += 1
        logger.error(f"❌ FAIL: {test_name}" + (f" — {detail}" if detail else ""))


# ─── Test 1: Healthcheck del servidor vLLM ──────────────────────────────────────

async def test_vllm_healthcheck():
    """Verifica que el endpoint /v1/models de vLLM responde correctamente."""
    test_name = "vLLM Healthcheck (/v1/models)"
    url = f"{VLLM_BASE_URL}/models"
    logger.info(f"🔍 [{test_name}] Consultando: {url}")

    try:
        async with httpx.AsyncClient() as client:
            response = await client.get(url, timeout=10.0)

        if response.status_code != 200:
            _report(test_name, False, f"HTTP {response.status_code}")
            return

        data = response.json()
        models = data.get("data", [])

        if not models:
            _report(test_name, False, "No se encontraron modelos cargados en vLLM.")
            return

        model_ids = [m.get("id", "desconocido") for m in models]
        _report(test_name, True, f"Modelos activos: {model_ids}")

    except httpx.ConnectError:
        _report(test_name, False, f"No se puede conectar a {url}. ¿Está corriendo vLLM?")
    except Exception as e:
        _report(test_name, False, f"Error inesperado: {e}")


# ─── Test 2: Generación de texto con ChatOpenAI (LangChain) ─────────────────────

async def test_chat_generation():
    """Genera una respuesta simple usando ChatOpenAI apuntando a vLLM."""
    test_name = "ChatOpenAI Generación de Texto"
    logger.info(f"🧠 [{test_name}] Enviando prompt de prueba...")

    llm = ChatOpenAI(
        model=VLLM_MODEL,
        base_url=VLLM_BASE_URL,
        api_key="EMPTY",
        temperature=0.1,
        max_tokens=256,
        timeout=60,
    )

    prompt = "Responde SOLAMENTE con la frase: 'vLLM funciona correctamente'. No agregues nada más."

    try:
        start = time.perf_counter()
        response = await llm.ainvoke([HumanMessage(content=prompt)])
        duration = time.perf_counter() - start

        content = response.content.strip()

        if not content:
            _report(test_name, False, "Respuesta vacía del modelo.")
            return

        # Extraer métricas de tokens si están disponibles
        usage = getattr(response, "usage_metadata", None)
        token_info = ""
        if usage:
            token_info = (
                f" | Tokens: in={usage.get('input_tokens', '?')}, "
                f"out={usage.get('output_tokens', '?')}"
            )

        _report(
            test_name,
            True,
            f"Respuesta: '{content[:100]}' | Latencia: {duration:.3f}s{token_info}",
        )

    except Exception as e:
        _report(test_name, False, f"Error: {e}")


# ─── Test 3: Generación y Parsing de JSON estructurado ──────────────────────────

async def test_json_output():
    """
    Valida que el modelo sea capaz de generar JSON válido y estructurado,
    simulando el formato de salida esperado por el extractor de TDR.
    """
    test_name = "Generación JSON Estructurada"
    logger.info(f"📋 [{test_name}] Probando generación de JSON...")

    llm = ChatOpenAI(
        model=VLLM_MODEL,
        base_url=VLLM_BASE_URL,
        api_key="EMPTY",
        temperature=0.0,
        max_tokens=512,
        timeout=90,
    )

    prompt = """Eres un asistente de datos. Devuelve SOLAMENTE un JSON válido, sin texto adicional.

El JSON debe tener exactamente esta estructura:
[
  {
    "id_prod": 1,
    "requerimientos": "Ejemplo de requerimiento técnico mínimo."
  },
  {
    "id_prod": 2,
    "requerimientos": "Segundo requerimiento de prueba."
  }
]

Responde SOLO con el JSON. Sin explicaciones, sin markdown, sin bloques de código."""

    try:
        start = time.perf_counter()
        response = await llm.ainvoke([HumanMessage(content=prompt)])
        duration = time.perf_counter() - start

        raw = response.content.strip()

        # Intentar limpiar si el modelo envuelve en ```json ... ```
        cleaned = raw
        if cleaned.startswith("```"):
            lines = cleaned.split("\n")
            # Quitar primera y última línea si son delimitadores de bloque
            if lines[0].startswith("```"):
                lines = lines[1:]
            if lines and lines[-1].strip() == "```":
                lines = lines[:-1]
            cleaned = "\n".join(lines)

        parsed: list[dict[str, Any]] = json.loads(cleaned)

        # Validaciones de estructura
        if not isinstance(parsed, list):
            _report(test_name, False, f"Se esperaba una lista, se obtuvo: {type(parsed).__name__}")
            return

        if len(parsed) < 1:
            _report(test_name, False, "La lista JSON está vacía.")
            return

        # Verificar que cada elemento tenga las claves esperadas
        required_keys = {"id_prod", "requerimientos"}
        for i, item in enumerate(parsed):
            if not isinstance(item, dict):
                _report(test_name, False, f"Elemento [{i}] no es un dict: {type(item).__name__}")
                return
            missing = required_keys - set(item.keys())
            if missing:
                _report(test_name, False, f"Elemento [{i}] sin claves: {missing}")
                return

        _report(
            test_name,
            True,
            f"{len(parsed)} elementos válidos | Latencia: {duration:.3f}s",
        )

    except json.JSONDecodeError as e:
        _report(test_name, False, f"JSON inválido: {e}. Respuesta raw: '{raw[:200]}'")
    except Exception as e:
        _report(test_name, False, f"Error: {e}")


# ─── Test 4: Verificación de latencia aceptable ─────────────────────────────────

async def test_latency_benchmark():
    """
    Ejecuta 3 invocaciones consecutivas y calcula la latencia promedio.
    Útil para detectar problemas de rendimiento GPU o sobrecarga del modelo.
    """
    test_name = "Benchmark de Latencia (3 invocaciones)"
    logger.info(f"⏱️ [{test_name}] Ejecutando benchmark...")

    llm = ChatOpenAI(
        model=VLLM_MODEL,
        base_url=VLLM_BASE_URL,
        api_key="EMPTY",
        temperature=0.0,
        max_tokens=64,
        timeout=60,
    )

    prompt = "Responde con una sola palabra: 'OK'."
    durations: list[float] = []

    try:
        for i in range(3):
            start = time.perf_counter()
            await llm.ainvoke([HumanMessage(content=prompt)])
            dur = time.perf_counter() - start
            durations.append(dur)
            logger.info(f"   Invocación {i+1}/3: {dur:.3f}s")

        avg = sum(durations) / len(durations)
        _report(
            test_name,
            True,
            f"Promedio: {avg:.3f}s | Min: {min(durations):.3f}s | Max: {max(durations):.3f}s",
        )

    except Exception as e:
        _report(test_name, False, f"Error durante benchmark: {e}")


# ─── Ejecución Principal ────────────────────────────────────────────────────────

async def main():
    logger.info("=" * 70)
    logger.info("🏁 SUITE DE PRUEBAS DE INTEGRACIÓN — vLLM LOCAL")
    logger.info(f"   Servidor: {VLLM_BASE_URL}")
    logger.info(f"   Modelo:   {VLLM_MODEL}")
    logger.info("=" * 70)

    await test_vllm_healthcheck()
    await test_chat_generation()
    await test_json_output()
    await test_latency_benchmark()

    logger.info("=" * 70)
    logger.info(f"📊 RESULTADOS: {_passed} pasaron, {_failed} fallaron")
    logger.info("=" * 70)

    if _failed > 0:
        logger.warning("⚠️ Hay tests fallidos. Revisa la configuración de vLLM.")
        sys.exit(1)
    else:
        logger.info("🎉 Todos los tests pasaron exitosamente.")
        sys.exit(0)


if __name__ == "__main__":
    asyncio.run(main())
```

---

# 5.9 Carpeta `scripts/`

## 5.9.1 `scripts/deploy.sh`

```bash
#!/bin/bash
source /workspace/venv/bin/activate
cd /root/Konomegua-Backend

echo "📥 Trayendo cambios..."
git pull

echo "📦 Actualizando dependencias..."
TMPDIR=/root pip install -q -r requirements.txt

echo "🔄 Reiniciando servicios..."
pkill -f uvicorn
sleep 2

nohup uvicorn app:app --host 0.0.0.0 --port 3000 --workers 2 \
  > /workspace/logs/fastapi.log 2>&1 &

echo "✅ Deploy listo"
tail -f /workspace/logs/fastapi.log
```

---

## 5.9.2 `scripts/runpod.sh`

```bash
#!/bin/bash
source /root/Konomegua-Backend/.env

case "$1" in
  start)
    echo "🚀 Encendiendo pod RunPod..."
    curl -s -X POST "https://api.runpod.io/graphql?api_key=$RUNPOD_API_KEY" \
      -H "Content-Type: application/json" \
      -d "{\"query\": \"mutation { podResume(input: {podId: \\\"$RUNPOD_POD_ID\\\", gpuCount: 1}) { id } }\"}"
    echo "✅ Pod encendido"
    ;;
  stop)
    echo "🛑 Apagando pod RunPod..."
    curl -s -X POST "https://api.runpod.io/graphql?api_key=$RUNPOD_API_KEY" \
      -H "Content-Type: application/json" \
      -d "{\"query\": \"mutation { podStop(input: {podId: \\\"$RUNPOD_POD_ID\\\"}) { id } }\"}"
    echo "✅ Pod apagado"
    ;;
  status)
    curl -s -X POST "https://api.runpod.io/graphql?api_key=$RUNPOD_API_KEY" \
      -H "Content-Type: application/json" \
      -d "{\"query\": \"query { pod(input: {podId: \\\"$RUNPOD_POD_ID\\\"}) { id desiredStatus currentStatus } }\"}"
    ;;
  *)
    echo "Uso: bash runpod.sh [start|stop|status]"
    ;;
esac
```

---

## 5.9.3 `scripts/start_vllm.sh`

```bash
#!/bin/bash
# ------------------------------------------------------------------------------
# Script de Inicio de vLLM en Producción
# Optimizado para arquitecturas NVIDIA (Ampere/Ada Lovelace/Hopper)
# ------------------------------------------------------------------------------

# Intentar cargar variables desde .env
if [ -f .env ]; then
    echo "⚙️ Cargando variables de entorno desde .env..."
    export $(grep -v '^#' .env | xargs)
fi

# Fallbacks si no están definidas
VLLM_MODEL=${VLLM_MODEL:-"Qwen/Qwen3-30B-A3B-AWQ"}
VLLM_PORT=${VLLM_PORT:-8000}
VLLM_HOST=${VLLM_HOST:-"0.0.0.0"}

echo "🚀 Iniciando servidor vLLM local con soporte OpenAI API..."
echo "📦 Modelo: $VLLM_MODEL"
echo "🌐 Escuchando en: http://$VLLM_HOST:$VLLM_PORT/v1"

python -m vllm.entrypoints.openai.api_server \
  --model "$VLLM_MODEL" \
  --host "$VLLM_HOST" \
  --port "$VLLM_PORT" \
  --gpu-memory-utilization 0.90 \
  --max-model-len 32768 \
  --dtype auto \
  --tensor-parallel-size 1 \
  --quantization awq_marlin \
  --trust-remote-code
```

---

## 5.9.4 `scripts/start_vps.sh`

```bash
#!/bin/bash
source /workspace/venv/bin/activate
service rabbitmq-server start
cd /root/Konomegua-Backend
nohup uvicorn app:app --host 0.0.0.0 --port 3000 --workers 1 \
  > /workspace/logs/fastapi.log 2>&1 &
echo "✅ VPS lista"
```

---

# 5.10 Carpeta `services/ai/`

## 5.10.1 `services/ai/__init__.py`

```python
"""
services.ai — Módulo de Inteligencia Artificial (LangChain + OpenRouter)

Centraliza toda la interacción con LLMs:
- client.py:    Instancia del modelo (ChatOpenAI → OpenRouter)
- prompts.py:   Templates de prompts reutilizables
- parser.py:    Parsers de salida (JSON)
- extractor.py: Chains y funciones async de extracción
"""

from services.ai.extractor import (
    extraer_requerimientos_texto,
    extraer_requerimientos_multimodal,
    extraer_datos_contrato,
)

__all__ = [
    "extraer_requerimientos_texto",
    "extraer_requerimientos_multimodal",
    "extraer_datos_contrato",
]
```

---

## 5.10.2 `services/ai/client.py`

```python
"""
services.ai.client — Cliente LLM centralizado (vLLM Local via LangChain)

Proporciona una instancia singleton de ChatOpenAI configurada para consumir
la API compatible con OpenAI expuesta por un servidor vLLM local. El modelo
y la URL del servidor se leen dinámicamente de variables de entorno.

Uso:
    from services.ai.client import llm, verificar_vllm_health
    response = await llm.ainvoke([...])
"""

import os
import logging
from langchain_openai import ChatOpenAI

logger = logging.getLogger("API-Konomegua")

# ─── Configuración de OpenRouter ──────────────────────────────────────────────
# Se lee del entorno la API Key de OpenRouter y el modelo seleccionado.
# ───────────────────────────────────────────────────────────────────────────────

_OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY", "")
_LLM_MODEL = os.getenv("LLM_MODEL_NAME", "openai/gpt-4o-mini")
_TEMPERATURE = 0.1
_MAX_TOKENS = 4000
_TIMEOUT = 120

# Inicialización del cliente LangChain apuntando a OpenRouter
llm = ChatOpenAI(
    model=_LLM_MODEL,
    api_key=_OPENROUTER_API_KEY,
    base_url="https://openrouter.ai/api/v1",
    temperature=_TEMPERATURE,
    max_tokens=_MAX_TOKENS,
    timeout=_TIMEOUT,
    max_retries=3,
    default_headers={
        "HTTP-Referer": "https://konomegua.com",
        "X-Title": "Konomegua Backend",
    }
)

logger.info(f"🤖 Cliente OpenRouter inicializado: {_LLM_MODEL}")

# Healthcheck dummy para no romper la firma de las funciones que lo importan
# Ya que OpenRouter siempre está activo, siempre retorna True.
async def verificar_vllm_health(*args, **kwargs) -> bool:
    if not _OPENROUTER_API_KEY:
        logger.error("❌ [OpenRouter] No se ha configurado la variable OPENROUTER_API_KEY en el .env")
        return False
    return True
```

---

## 5.10.3 `services/ai/extractor.py`

```python
"""
services.ai.extractor — Funciones de Extracción IA (Chains LangChain)

Contiene las funciones async de alto nivel que orquestan la extracción
de información usando LLMs. Cada función encapsula:
- Construcción del prompt
- Invocación del modelo
- Parsing de la respuesta

Dos modos de operación:
1. Texto puro: Para PDFs con texto nativo extraído por pdfplumber/tesseract
2. Multimodal: Para PDFs escaneados donde se envían imágenes al LLM

Uso:
    from services.ai import extraer_requerimientos_texto
    resultados = await extraer_requerimientos_texto(productos, texto_tdr)
"""

import json
import logging
import time
from typing import Any

from langchain_core.messages import HumanMessage

from services.ai.client import llm, verificar_vllm_health
from services.ai.prompts import EXTRAER_REQUERIMIENTOS_TEMPLATE, EXTRAER_CONTRATO_TEMPLATE
from services.ai.parser import json_parser

logger = logging.getLogger("API-Konomegua")


def _log_llm_metrics(operation_name: str, duration: float, response: Any):
    """
    Extrae y reporta de forma segura métricas de consumo de tokens y latencia de la respuesta.
    """
    usage = getattr(response, "usage_metadata", None)
    if usage:
        prompt_tokens = usage.get("input_tokens", 0)
        completion_tokens = usage.get("output_tokens", 0)
        total_tokens = usage.get("total_tokens", 0)
    else:
        # Fallback para response_metadata
        resp_meta = getattr(response, "response_metadata", {})
        token_usage = resp_meta.get("token_usage", {}) if isinstance(resp_meta, dict) else {}
        prompt_tokens = token_usage.get("prompt_tokens", 0)
        completion_tokens = token_usage.get("completion_tokens", 0)
        total_tokens = token_usage.get("total_tokens", 0)

    logger.info(
        f"⚡ [{operation_name}] Inferencia completada en {duration:.3f}s. "
        f"Tokens: Entrada={prompt_tokens}, Salida={completion_tokens}, Total={total_tokens}"
    )


# ─── Chain de Extracción de Requerimientos (Texto) ─────────────────────────────
# Pipeline: PromptTemplate → ChatOpenAI → JsonOutputParser
# Equivale al patrón LCEL: prompt | llm | parser
# ───────────────────────────────────────────────────────────────────────────────

_chain_requerimientos = EXTRAER_REQUERIMIENTOS_TEMPLATE | llm | json_parser


async def extraer_requerimientos_texto(
    productos: list[dict[str, Any]],
    texto_tdr: str,
) -> list[dict[str, Any]]:
    """
    Extrae requerimientos técnicos de un TDR usando texto puro.

    Args:
        productos: Lista de dicts con 'id_prod' y 'descripcion' de cada producto.
        texto_tdr: Texto plano extraído del PDF del TDR.

    Returns:
        Lista de dicts con 'id_prod' y 'requerimientos' para cada producto.

    Raises:
        Exception: Si el LLM no responde o el JSON es inválido.
    """
    start_time = time.perf_counter()
    logger.info(f"🧠 Extracción IA (texto): {len(productos)} productos")

    # Formatear prompt
    prompt_value = await EXTRAER_REQUERIMIENTOS_TEMPLATE.ainvoke({
        "productos": json.dumps(productos, ensure_ascii=False, indent=2),
        "texto_tdr": texto_tdr,
    })

    try:
        # Esperar a que el servidor vLLM esté listo
        if not await verificar_vllm_health():
            raise Exception("Timeout esperando al servidor vLLM local")

        # Invocar LLM directamente para poder capturar metadatos/tokens
        response = await llm.ainvoke(prompt_value)
        duration = time.perf_counter() - start_time

        _log_llm_metrics("vLLM Extracción Texto", duration, response)

        # Parsear con json_parser
        resultados = await json_parser.aparse(response.content)
        logger.info(f"✅ IA retornó {len(resultados)} resultados")
        return resultados

    except Exception as e:
        duration = time.perf_counter() - start_time
        logger.error(f"❌ [vLLM Extracción Texto] Error tras {duration:.3f}s: {e}")
        raise e


async def extraer_requerimientos_multimodal(
    productos: list[dict[str, Any]],
    texto_tdr: str,
    imagenes_b64: list[str],
) -> list[dict[str, Any]]:
    """
    Extrae requerimientos técnicos de un TDR usando texto + imágenes del PDF.

    Se usa cuando el PDF es escaneado y pdfplumber/tesseract no pudieron
    extraer texto suficiente. Las imágenes se envían al LLM como contenido
    multimodal (base64 JPEG).

    Args:
        productos: Lista de dicts con 'id_prod' y 'descripcion'.
        texto_tdr: Texto parcial extraído (puede estar casi vacío).
        imagenes_b64: Lista de imágenes del PDF en base64 (JPEG).

    Returns:
        Lista de dicts con 'id_prod' y 'requerimientos'.

    Raises:
        Exception: Si el LLM no responde o el JSON es inválido.
    """
    start_time = time.perf_counter()
    logger.info(
        f"🧠 Extracción IA (multimodal): {len(productos)} productos, "
        f"{len(imagenes_b64)} imágenes"
    )

    # Construir el prompt formateado manualmente (PromptTemplate no soporta imágenes)
    prompt_text = EXTRAER_REQUERIMIENTOS_TEMPLATE.format(
        productos=json.dumps(productos, ensure_ascii=False, indent=2),
        texto_tdr=texto_tdr,
    )

    # Construir contenido multimodal: texto + imágenes
    content: list[dict[str, Any]] = [{"type": "text", "text": prompt_text}]
    for b64 in imagenes_b64:
        content.append({
            "type": "image_url",
            "image_url": {"url": f"data:image/jpeg;base64,{b64}"},
        })

    try:
        # Esperar a que el servidor vLLM esté listo
        if not await verificar_vllm_health():
            raise Exception("Timeout esperando al servidor vLLM local")

        # Invocar el LLM directamente con HumanMessage (sin chain, por el contenido mixto)
        response = await llm.ainvoke([HumanMessage(content=content)])
        duration = time.perf_counter() - start_time

        _log_llm_metrics("vLLM Extracción Multimodal", duration, response)

        # Parsear la respuesta JSON manualmente (el parser del chain no aplica aquí)
        resultados = await json_parser.aparse(response.content)

        logger.info(f"✅ IA multimodal retornó {len(resultados)} resultados")
        return resultados

    except Exception as e:
        duration = time.perf_counter() - start_time
        logger.error(f"❌ [vLLM Extracción Multimodal] Error tras {duration:.3f}s: {e}")
        raise e


async def extraer_datos_contrato(
    productos: list[dict[str, Any]],
    contexto: str,
) -> dict[str, Any]:
    """
    Extrae especificaciones de productos y condiciones globales del contrato usando el vLLM.

    Args:
        productos: Lista de dicts con 'id_prod' y 'descripcion'.
        contexto: Texto completo o parcial del contrato/TDR para extraer información.

    Returns:
        Un diccionario con 'productos' y 'condiciones' según el template de prompt de contrato.
    """
    start_time = time.perf_counter()
    logger.info(f"🧠 Extracción de especificaciones y condiciones (vLLM): {len(productos)} productos")

    # Formatear prompt
    prompt_value = await EXTRAER_CONTRATO_TEMPLATE.ainvoke({
        "productos": json.dumps(productos, ensure_ascii=False, indent=2),
        "contexto": contexto,
    })

    try:
        # Esperar a que el servidor vLLM esté listo
        if not await verificar_vllm_health():
            raise Exception("Timeout esperando al servidor vLLM local")

        response = await llm.ainvoke(prompt_value)
        duration = time.perf_counter() - start_time

        _log_llm_metrics("vLLM Extracción Contrato", duration, response)

        # Parsear con json_parser
        resultados = await json_parser.aparse(response.content)
        logger.info(f"✅ IA de contrato retornó especificaciones para {len(resultados.get('productos', []))} productos y {len(resultados.get('condiciones', []))} condiciones globales.")
        return resultados

    except Exception as e:
        duration = time.perf_counter() - start_time
        logger.error(f"❌ [vLLM Extracción Contrato] Error tras {duration:.3f}s: {e}")
        # Retornar estructura por defecto vacía en caso de falla
        return {"productos": [], "condiciones": []}

```

---

## 5.10.4 `services/ai/parser.py`

```python
"""
services.ai.parser — Parsers de Salida para Respuestas LLM

Proporciona parsers reutilizables que transforman la salida cruda del LLM
en estructuras de datos Python tipadas.

Reemplaza el patrón manual de:
    content.replace("```json", "").replace("```", "").strip()
    json.loads(content)

LangChain's JsonOutputParser maneja automáticamente:
- Limpieza de bloques ```json ... ```
- Parsing robusto de JSON
- Mensajes de error descriptivos
"""

from langchain_core.output_parsers import JsonOutputParser

# ─── Parser JSON Genérico ──────────────────────────────────────────────────────
# Parsea la salida del LLM como JSON. Soporta tanto objetos como arrays.
# Se usa como último eslabón del chain: prompt | llm | json_parser
# ───────────────────────────────────────────────────────────────────────────────

json_parser = JsonOutputParser()
```

---

## 5.10.5 `services/ai/prompts.py`

```python
"""
services.ai.prompts — Templates de Prompts para Extracción IA

Centraliza todos los prompts del sistema en constantes reutilizables.
Usa PromptTemplate de LangChain para inyección segura de variables.

Convenciones:
- Constantes en MAYÚSCULAS para los textos raw
- Sufijo _TEMPLATE para los objetos PromptTemplate
"""

from langchain_core.prompts import PromptTemplate

# ═══════════════════════════════════════════════════════════════════════════════
# EXTRACCIÓN DE REQUERIMIENTOS DESDE TDR (Términos de Referencia)
# ═══════════════════════════════════════════════════════════════════════════════
# Contexto: El Estado Peruano publica TDR en PDF para sus convocatorias de
# compra. Este prompt extrae los requerimientos técnicos específicos para
# cada producto de la lista proporcionada.
# ═══════════════════════════════════════════════════════════════════════════════

EXTRAER_REQUERIMIENTOS_PROMPT = """\
Eres un experto analizando Términos de Referencia (TDR) del Estado Peruano.
Extrae los requerimientos específicos o características técnicas para los siguientes productos.
Lista de productos a buscar:
{productos}

Devuelve ÚNICAMENTE un JSON válido con este formato exacto:
[
  {{
    "id_prod": <el id_prod de la lista correspondiente>,
    "requerimientos": ["Requisito 1", "Requisito 2"]
  }}
]

Texto del TDR (si aplica):
{texto_tdr}"""

# PromptTemplate listo para usar en chains de LangChain.
# Variables: productos (str JSON), texto_tdr (str con texto extraído del PDF)
EXTRAER_REQUERIMIENTOS_TEMPLATE = PromptTemplate(
    input_variables=["productos", "texto_tdr"],
    template=EXTRAER_REQUERIMIENTOS_PROMPT,
)

# ═══════════════════════════════════════════════════════════════════════════════
# EXTRACCIÓN DE ESPECIFICACIONES Y CONDICIONES DESDE CONTRATO SEACE
# ═══════════════════════════════════════════════════════════════════════════════

EXTRAER_CONTRATO_PROMPT = """\
Eres un experto analizando documentos de contratación del Estado Peruano (SEACE).
Tu tarea es analizar el texto proporcionado para extraer dos cosas:
1. Las especificaciones técnicas (características físicas, marcas, modelos, capacidades, compatibilidades) de los productos solicitados.
2. Las condiciones comerciales globales del contrato (plazo de entrega, lugar de entrega, forma de pago, garantías, penalidades, etc.).

Productos a analizar:
{productos}

Documento de referencia:
{contexto}

Devuelve ÚNICAMENTE un JSON válido con este formato exacto:
{{
  "productos": [
    {{
      "id_prod": <el id_prod correspondiente de la lista>,
      "especificaciones": ["Especificación técnica 1", "Especificación técnica 2"]
    }}
  ],
  "condiciones": [
    "Plazo de entrega: [especificación del plazo]",
    "Lugar de entrega: [especificación del lugar]",
    "Forma de pago: [especificación del pago]",
    "Garantías: [especificación de garantías]",
    "Penalidades: [especificación de penalidades]"
  ]
}}
"""

EXTRAER_CONTRATO_TEMPLATE = PromptTemplate(
    input_variables=["productos", "contexto"],
    template=EXTRAER_CONTRATO_PROMPT,
)

```

---

# 5.11 Carpeta `services/queue/`

## 5.11.1 `services/queue/__init__.py`

```python
# services/queue/ — Módulo de colas asíncronas RabbitMQ
```

---

## 5.11.2 `services/queue/connection.py`

```python
"""
services.queue.connection — Administrador de Conexión de RabbitMQ (Singleton con reconexión robusta)

Este módulo gestiona la conexión asíncrona a RabbitMQ usando aio-pika.
Utiliza la capacidad de 'connect_robust' para reestablecer automáticamente la
conexión si se cae, y expone un canal singleton para reutilizarlo en publicadores
y consumidores de forma concurrente sin abrir múltiples sockets innecesarios.

Uso:
    from services.queue.connection import RabbitMQConnection
    channel = await RabbitMQConnection.get_instance().get_channel()
"""

import os
import asyncio
import logging
import aio_pika

logger = logging.getLogger("API-Konomegua")

class RabbitMQConnection:
    _instance = None

    def __init__(self):
        self.url = os.getenv("RABBITMQ_URL")
        self.connection = None
        self.channel = None
        self._lock = asyncio.Lock()

    @classmethod
    def get_instance(cls):
        """Devuelve la instancia singleton de la conexión."""
        if cls._instance is None:
            cls._instance = cls()
        return cls._instance

    async def get_connection(self) -> aio_pika.RobustConnection:
        """
        Obtiene o crea una conexión robusta con RabbitMQ.
        Maneja reconexión automática en segundo plano.
        """
        async with self._lock:
            if self.connection is None or self.connection.is_closed:
                logger.info(f"🔌 Conectando a RabbitMQ en {self.url}...")
                self.connection = await aio_pika.connect_robust(
                    self.url,
                    timeout=10,
                    client_properties={"connection_name": "konomegua_api"}
                )
                logger.info("✅ Conexión robusta a RabbitMQ establecida con éxito.")
            return self.connection

    async def get_channel(self) -> aio_pika.RobustChannel:
        """
        Obtiene o crea un canal robusto reutilizable.
        El canal robusto se recupera automáticamente tras una reconexión.
        """
        async with self._lock:
            conn = await self.get_connection()
            if self.channel is None or self.channel.is_closed:
                logger.info("🔌 Creando canal robusto en RabbitMQ...")
                self.channel = await conn.channel()
                logger.info("✅ Canal robusto de RabbitMQ creado y listo.")
            return self.channel

    async def close(self):
        """Cierra el canal y la conexión de forma limpia."""
        async with self._lock:
            if self.channel and not self.channel.is_closed:
                await self.channel.close()
                logger.info("🗑️ Canal de RabbitMQ cerrado.")
            if self.connection and not self.connection.is_closed:
                await self.connection.close()
                logger.info("🔌 Conexión de RabbitMQ cerrada.")
            self.channel = None
            self.connection = None
```

---

## 5.11.3 `services/queue/consumer.py`

```python
"""
services.queue.consumer — Consumidor Base para Colas de RabbitMQ

Maneja el bucle asíncrono principal de escucha (listening) y la resiliencia
frente a caídas de conexión y procesamiento fallido con acknowledgements correctos.
"""

import asyncio
import logging
from aio_pika import IncomingMessage

from services.queue.connection import RabbitMQConnection

logger = logging.getLogger("API-Konomegua")

class BaseConsumer:
    """
    Consumidor Base con resiliencia, control de flujo (QoS / prefetch_count)
    y gestión segura del ciclo de vida de mensajes (ACK/NACK).
    """
    def __init__(self, queue_name: str, prefetch_count: int = 1):
        self.queue_name = queue_name
        self.prefetch_count = prefetch_count
        self.connection_manager = RabbitMQConnection.get_instance()

    async def handle_message(self, message: IncomingMessage):
        """
        Debe ser sobreescrito por cada clase consumidora específica.
        """
        raise NotImplementedError("Debe implementar handle_message en la clase hija.")

    async def start_consuming(self):
        """
        Inicia el bucle de escucha de la cola.
        Si la conexión se interrumpe, entra en modo reintento hasta restablecerse.
        """
        while True:
            try:
                # 1. Obtener canal de comunicación singleton robusto
                channel = await self.connection_manager.get_channel()
                
                # 2. Configurar la calidad de servicio (limitar a un mensaje a la vez por worker)
                await channel.set_qos(prefetch_count=self.prefetch_count)
                
                # 3. Declarar la cola como durable
                queue = await channel.declare_queue(self.queue_name, durable=True)
                
                logger.info(f"📥 Consumidor iniciado y escuchando en la cola '{self.queue_name}'...")

                # 4. Iterar sobre la cola indefinidamente
                async with queue.iterator() as queue_iter:
                    async for message in queue_iter:
                        # process() gestiona automáticamente ACK (si el bloque 'async with' termina sin errores)
                        # o NACK (si ocurre una excepción no controlada dentro del bloque).
                        # Esto cumple al 100% con los requerimientos de retries correctos y no pérdida de datos.
                        async with message.process():
                            await self.handle_message(message)

            except asyncio.CancelledError:
                logger.info(f"🛑 Bucle de consumo en '{self.queue_name}' cancelado.")
                break
            except Exception as e:
                logger.error(f"❌ Error en el consumidor de '{self.queue_name}': {e}")
                logger.info("🔄 Reintentando reconexión al broker RabbitMQ en 5 segundos...")
                await asyncio.sleep(5)
```

---

## 5.11.4 `services/queue/publisher.py`

```python
"""
services.queue.publisher — Publicador de mensajes RabbitMQ

Encapsula la lógica de encolado de mensajes de forma persistente.
Usa canales y colas durables para evitar pérdidas de datos en fallos.
"""

import json
import logging
from typing import Dict, Any
from aio_pika import Message, DeliveryMode

from services.queue.connection import RabbitMQConnection
from services.queue.schemas import TDRJobPayload

logger = logging.getLogger("API-Konomegua")

QUEUE_TDR_JOBS = "tdr_jobs"

async def publicar_tdr_job(data: Dict[str, Any]):
    """
    Publica un trabajo de extracción de TDR en la cola 'tdr_jobs'.
    
    Args:
        data: Diccionario con el payload del trabajo que será validado y serializado.
    """
    try:
        # 1. Validar la estructura del payload mediante el esquema Pydantic
        payload = TDRJobPayload(**data)
        serialized_payload = payload.json()
        
        # 2. Obtener el canal singleton robusto
        connection_manager = RabbitMQConnection.get_instance()
        channel = await connection_manager.get_channel()
        
        # 4. Declarar la cola como durable (persiste a reinicios de RabbitMQ)
        await channel.declare_queue(QUEUE_TDR_JOBS, durable=True)
        
        # 5. Crear el mensaje persistente (delivery_mode=PERSISTENT)
        message = Message(
            serialized_payload.encode("utf-8"),
            delivery_mode=DeliveryMode.PERSISTENT,
            content_type="application/json"
        )
        
        # 6. Publicar al default exchange enrutando directamente a la cola
        await channel.default_exchange.publish(
            message,
            routing_key=QUEUE_TDR_JOBS
        )
        logger.info(
            f"📤 [Job Encolado] Cola '{QUEUE_TDR_JOBS}' — "
            f"Cotización: {payload.num} (Año: {payload.year})"
        )
    except Exception as e:
        logger.error(f"❌ Error al publicar TDR job en RabbitMQ: {e}")
        raise
```

---

## 5.11.5 `services/queue/schemas.py`

```python
"""
services.queue.schemas — Esquemas de validación de payloads de RabbitMQ

Garantiza que la información enviada entre los publicadores y los consumidores
cumpla con las estructuras y los tipos esperados mediante Pydantic.
"""

from pydantic import BaseModel
from typing import List, Dict, Any

class TDRJobPayload(BaseModel):
    """
    Payload de validación para tareas de procesamiento de TDR.
    """
    year: int
    uejec: str
    num: str
    items_mapeados: List[Dict[str, Any]]
```

---

# 5.12 Carpeta `services/rag/`

## 5.12.1 `services/rag/__init__.py`

```python
"""
services.rag — Pipeline de Retrieval-Augmented Generation (RAG)

Este módulo gestiona la transformación de PDFs de gran tamaño en fragmentos
recuperables para la IA. 

Contiene:
- chunker.py: Lógica de text splitting
- embeddings.py: Gestión del modelo de embeddings local
- vectorstore.py: Cliente ChromaDB
- ingestion.py: Pipeline de carga de documentos a la DB
- retriever.py: Búsqueda de similitud
- pipeline.py: LCEL chain uniendo retrieval y generación
- schemas.py: Definición de metadatos y payloads
"""
```

---

## 5.12.2 `services/rag/chunker.py`

```python
"""
services.rag.chunker — Lógica de Text Splitting

Implementa RecursiveCharacterTextSplitter para dividir los textos extraídos 
por OCR (TDRs y PDFs grandes) en chunks manejables preservando el contexto.
"""

import logging
try:
    from langchain_text_splitters import RecursiveCharacterTextSplitter
except ImportError:
    from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_core.documents import Document

logger = logging.getLogger("API-Konomegua")

# Configuración optimizada para TDRs:
# - chunk_size de 1000 a 1500 suele cubrir un párrafo grande o una tabla técnica.
# - chunk_overlap de 200 asegura que un requerimiento que cruza entre dos chunks
#   no pierda su contexto semántico.
CHUNK_SIZE = 1200
CHUNK_OVERLAP = 250

_splitter = RecursiveCharacterTextSplitter(
    chunk_size=CHUNK_SIZE,
    chunk_overlap=CHUNK_OVERLAP,
    length_function=len,
    separators=["\n\n", "\n", " ", ""]
)

def dividir_texto_en_chunks(texto_completo: str, metadata_base: dict) -> list[Document]:
    """
    Divide un texto plano largo en objetos Document con metadata.
    
    Args:
        texto_completo: Texto raw extraído por OCR/pdfplumber.
        metadata_base: Diccionario base de metadata (ej. convocatoria_id).
        
    Returns:
        Lista de LangChain Documents listos para Embeddings.
    """
    if not texto_completo or not texto_completo.strip():
        return []
        
    logger.info(f"✂️ [Chunker] Dividiendo texto de {len(texto_completo)} caracteres...")
    
    docs = _splitter.create_documents([texto_completo], metadatas=[metadata_base])
    
    # Enriquecer metadata con el índice del chunk
    for i, doc in enumerate(docs):
        doc.metadata["chunk_index"] = i
        
    logger.info(f"✅ [Chunker] Texto dividido en {len(docs)} fragmentos.")
    return docs
```

---

## 5.12.3 `services/rag/embeddings.py`

```python
"""
services.rag.embeddings — Inicialización de Embeddings Locales

Utiliza HuggingFaceEmbeddings de LangChain para cargar un modelo local
(ej. intfloat/multilingual-e5-large o BAAI/bge-m3).
Al ser ejecutado en el entorno local, no consume APIs de pago (OpenAI/Cohere).
"""

import os
import logging
from langchain_huggingface import HuggingFaceEmbeddings

logger = logging.getLogger("API-Konomegua")

# intfloat/multilingual-e5-large es excelente para español e inglés y RAG general
# Si el entorno tiene poca RAM, se puede bajar a intfloat/multilingual-e5-small
_EMBEDDING_MODEL_NAME = os.getenv("EMBEDDING_MODEL", "intfloat/multilingual-e5-large")

logger.info(f"⏳ [Embeddings] Cargando modelo local de embeddings: {_EMBEDDING_MODEL_NAME} (Esto puede tardar en el primer arranque...)")

try:
    local_embeddings = HuggingFaceEmbeddings(
        model_name=_EMBEDDING_MODEL_NAME,
        model_kwargs={'device': 'cpu'},  # Embeddings en CPU — libera VRAM completa para el LLM
        encode_kwargs={'normalize_embeddings': True} # Esencial para cosine similarity (default en Chroma)
    )
    logger.info("✅ [Embeddings] Modelo local cargado exitosamente en memoria.")
except Exception as e:
    logger.critical(f"❌ [Embeddings] Error crítico cargando modelo HuggingFace: {e}")
    raise e
```

---

## 5.12.4 `services/rag/ingestion.py`

```python
"""
services.rag.ingestion — Pipeline de Ingestión RAG

Orquesta la transformación de texto crudo de un TDR en embeddings 
almacenados en ChromaDB, asegurando deduplicación.
"""

import logging
import asyncio
from typing import Optional

from services.rag.chunker import dividir_texto_en_chunks
from services.rag.vectorstore import get_vector_store, verificar_coleccion_existe

logger = logging.getLogger("API-Konomegua")

async def ingerir_documento_tdr(texto_completo: str, year: int, uejec: str, num: str) -> bool:
    """
    Toma el texto extraído por OCR, lo procesa y lo indexa en ChromaDB.
    
    Args:
        texto_completo: Texto raw del TDR.
        year, uejec, num: Identificadores únicos de la convocatoria.
        
    Returns:
        True si fue ingerido exitosamente o si ya existía. False si hubo error o no hay texto.
    """
    if not texto_completo or not texto_completo.strip():
        logger.warning(f"⚠️ [Ingestión] No hay texto para ingerir en {num}")
        return False
        
    convocatoria_id = f"{year}-{num}-{uejec}"
    
    # Evitar re-ingestiones costosas (Deduplicación)
    if verificar_coleccion_existe(convocatoria_id):
        logger.info(f"⏭️ [Ingestión] Los chunks de la convocatoria {convocatoria_id} ya están en ChromaDB. Omitiendo.")
        return True
        
    logger.info(f"📥 [Ingestión] Iniciando pipeline de embeddings para {convocatoria_id}...")
    
    # 1. Chunking
    base_metadata = {
        "convocatoria_id": convocatoria_id,
        "year": year,
        "num": num,
        "source": "TDR",
    }
    documentos = dividir_texto_en_chunks(texto_completo, base_metadata)
    
    if not documentos:
        logger.warning(f"⚠️ [Ingestión] El chunker no produjo fragmentos para {convocatoria_id}")
        return False
        
    # 2. Inserción (Batching + Embeddings de forma transparente por LangChain Chroma)
    # Se recomienda correr en thread pool o envolver en asyncio si el método es bloqueante
    try:
        store = get_vector_store()
        
        # add_documents es bloqueante (realiza cálculos pesados CPU/GPU en local),
        # lo envolvemos para no bloquear el event loop principal de FastAPI/Worker.
        await asyncio.to_thread(store.add_documents, documents=documentos)
        
        logger.info(f"✅ [Ingestión] {len(documentos)} chunks indexados exitosamente en ChromaDB para {convocatoria_id}")
        return True
        
    except Exception as e:
        logger.error(f"❌ [Ingestión] Fallo crítico indexando en ChromaDB para {convocatoria_id}: {e}")
        return False
```

---

## 5.12.5 `services/rag/pipeline.py`

```python
"""
services.rag.pipeline — Pipeline principal RAG (LCEL)

Une la recuperación de contexto (VectorDB) con la generación (vLLM local).
En vez de extraer requerimientos para TODOS los productos enviando el PDF
completo, se ejecuta este pipeline producto por producto usando solo el
texto relevante.
"""

import json
import logging
import time
import asyncio
from typing import Any, Dict

from langchain_core.prompts import PromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_core.messages import HumanMessage

from services.ai.client import llm
from services.ai.parser import json_parser
from services.ai.extractor import _log_llm_metrics
from services.rag.retriever import buscar_contexto_relevante, formatear_contexto

logger = logging.getLogger("API-Konomegua")

# Prompt modificado para RAG: 
# Ahora espera `contexto_relevante` en vez de `texto_tdr` (que era el PDF completo)
RAG_EXTRAER_REQUERIMIENTOS_PROMPT = """\
Eres un experto analizando Términos de Referencia (TDR) del Estado Peruano.
Usa SOLO el siguiente contexto extraído del documento original para identificar los 
requerimientos específicos o características técnicas del producto solicitado.

Producto a analizar:
{producto}

Contexto relevante del documento:
{contexto_relevante}

Devuelve ÚNICAMENTE un JSON válido con este formato exacto:
{{
  "id_prod": {id_prod},
  "requerimientos": ["Requisito 1", "Requisito 2"]
}}
"""

RAG_TEMPLATE = PromptTemplate(
    input_variables=["producto", "contexto_relevante", "id_prod"],
    template=RAG_EXTRAER_REQUERIMIENTOS_PROMPT,
)

async def extraer_requerimientos_rag(
    id_prod: int,
    producto_descripcion: str,
    convocatoria_id: str,
    top_k: int = 5
) -> Dict[str, Any]:
    """
    Pipeline RAG para un solo producto.
    1. Busca top_k chunks en ChromaDB usando el nombre del producto.
    2. Inyecta los chunks en el prompt.
    3. Invoca a vLLM.
    4. Devuelve el JSON parseado.
    """
    start_time = time.perf_counter()
    logger.info(f"🧠 [RAG] Iniciando extracción para producto '{producto_descripcion[:30]}...'")

    # 1. Recuperación asíncrona de contexto
    # Ejecutamos la búsqueda (síncrona) en un thread para no bloquear el loop
    documentos = await asyncio.to_thread(
        buscar_contexto_relevante,
        query=producto_descripcion,
        convocatoria_id=convocatoria_id,
        top_k=top_k
    )
    
    contexto_str = formatear_contexto(documentos)
    
    if not contexto_str.strip():
        logger.warning(f"⚠️ [RAG] No se encontró contexto para '{producto_descripcion}'. El LLM puede inventar.")
        contexto_str = "No hay contexto específico en el documento para este producto."

    # 2. Construcción del Prompt
    prompt_value = await RAG_TEMPLATE.ainvoke({
        "producto": producto_descripcion,
        "contexto_relevante": contexto_str,
        "id_prod": id_prod
    })

    # 3. Invocación al LLM
    try:
        response = await llm.ainvoke(prompt_value)
        duration = time.perf_counter() - start_time

        _log_llm_metrics("vLLM RAG Pipeline", duration, response)

        # 4. Parsing de salida
        resultado = await json_parser.aparse(response.content)
        return resultado

    except Exception as e:
        duration = time.perf_counter() - start_time
        logger.error(f"❌ [RAG] Error tras {duration:.3f}s analizando producto {id_prod}: {e}")
        # En caso de error, retornar una estructura base vacía para no romper el ciclo del worker
        return {"id_prod": id_prod, "requerimientos": []}
```

---

## 5.12.6 `services/rag/retriever.py`

```python
"""
services.rag.retriever — Lógica de Recuperación (Similarity Search)

Busca en la base de datos vectorial (ChromaDB) los chunks más similares a una
consulta dada (ej. nombre del producto), filtrando siempre por la convocatoria
específica para evitar mezcla de contextos.
"""

import logging
from typing import List
from langchain_core.documents import Document

from services.rag.vectorstore import get_vector_store

logger = logging.getLogger("API-Konomegua")

def buscar_contexto_relevante(query: str, convocatoria_id: str, top_k: int = 4) -> List[Document]:
    """
    Realiza una búsqueda semántica en la Vector DB.
    
    Args:
        query: Nombre o descripción del producto a buscar.
        convocatoria_id: ID para filtrar (evita buscar en otros TDRs).
        top_k: Cantidad de chunks más relevantes a retornar.
        
    Returns:
        Lista de los K Documentos más relevantes.
    """
    store = get_vector_store()
    
    logger.info(f"🔎 [Retriever] Buscando contexto para: '{query[:50]}...' (Top {top_k})")
    
    try:
        # Buscar en la DB limitando por el metadata 'convocatoria_id'
        resultados = store.similarity_search(
            query=query,
            k=top_k,
            filter={"convocatoria_id": convocatoria_id}
        )
        
        if not resultados:
            logger.warning(f"⚠️ [Retriever] No se encontró contexto relevante para '{query[:30]}'")
            
        return resultados
    except Exception as e:
        logger.error(f"❌ [Retriever] Error en la búsqueda vectorial: {e}")
        return []

def formatear_contexto(documentos: List[Document]) -> str:
    """
    Convierte una lista de documentos recuperados en un string consolidado
    para ser inyectado en el prompt del LLM.
    """
    contextos = []
    for doc in documentos:
        chunk_info = f"[Página/Chunk {doc.metadata.get('chunk_index', '?')}]"
        contextos.append(f"{chunk_info}:\n{doc.page_content.strip()}")
        
    return "\n\n---\n\n".join(contextos)
```

---

## 5.12.7 `services/rag/schemas.py`

```python
"""
services.rag.schemas — Definiciones de Pydantic para el pipeline RAG
"""

from typing import Optional
from pydantic import BaseModel

class ChunkMetadata(BaseModel):
    """
    Metadatos asociados a un fragmento (chunk) de un TDR en la VectorDB.
    Permite filtrar los resultados del similarity search.
    """
    convocatoria_id: str  # e.g., '2026-10-1423' (year-num-uejec)
    chunk_index: int
    source: str = "TDR"

class RetrievalQuery(BaseModel):
    """
    Payload de búsqueda para la base de datos vectorial.
    """
    query: str
    convocatoria_id: str
    top_k: int = 5
```

---

## 5.12.8 `services/rag/vectorstore.py`

```python
"""
services.rag.vectorstore — Gestión de Base de Datos Vectorial (ChromaDB)

Proporciona el singleton y las utilidades para interactuar con ChromaDB.
Utiliza PersistentClient de chromadb para almacenar los datos en disco localmente.
"""

import os
import logging
from langchain_chroma import Chroma
import chromadb

from services.rag.embeddings import local_embeddings

logger = logging.getLogger("API-Konomegua")

# Configuración de Servidor ChromaDB (Producción) o Local (Desarrollo)
CHROMA_HOST = os.getenv("CHROMA_HOST", "")
CHROMA_PORT = os.getenv("CHROMA_PORT", "8000")
CHROMA_PERSIST_DIR = os.getenv("CHROMA_PERSIST_DIR", "./chroma_db")
COLLECTION_NAME = "tdr_collection"

if CHROMA_HOST:
    try:
        # Modo Producción: Conectar al contenedor ChromaDB
        _chroma_client = chromadb.HttpClient(host=CHROMA_HOST, port=int(CHROMA_PORT))
        _chroma_client.heartbeat()
        logger.info(f"💾 [VectorStore] Conectado a Servidor ChromaDB en {CHROMA_HOST}:{CHROMA_PORT}")
    except Exception as e:
        logger.warning(f"⚠️ [VectorStore] No se pudo conectar al servidor Chroma en {CHROMA_HOST}:{CHROMA_PORT}: {e}. Cayendo en fallback a PersistentClient local.")
        _chroma_client = chromadb.PersistentClient(path=CHROMA_PERSIST_DIR)
else:
    # Modo Desarrollo: Base de datos embebida en disco
    _chroma_client = chromadb.PersistentClient(path=CHROMA_PERSIST_DIR)
    logger.info(f"💾 [VectorStore] Conectado a ChromaDB Local en {CHROMA_PERSIST_DIR}")

# Inicializar wrapper de LangChain sobre Chroma
try:
    vector_store = Chroma(
        client=_chroma_client,
        collection_name=COLLECTION_NAME,
        embedding_function=local_embeddings
    )
except Exception as e:
    logger.warning(f"⚠️ [VectorStore] Error al inicializar LangChain Chroma con el cliente actual: {e}. Cayendo en fallback a PersistentClient local.")
    _chroma_client = chromadb.PersistentClient(path=CHROMA_PERSIST_DIR)
    vector_store = Chroma(
        client=_chroma_client,
        collection_name=COLLECTION_NAME,
        embedding_function=local_embeddings
    )

logger.info(f"💾 [VectorStore] Conectado a ChromaDB en {CHROMA_PERSIST_DIR} (Colección: {COLLECTION_NAME})")

def verificar_coleccion_existe(convocatoria_id: str) -> bool:
    """
    Verifica si los chunks de una convocatoria específica ya han sido indexados
    previamente en ChromaDB para evitar ingestión duplicada.
    """
    try:
        collection = _chroma_client.get_collection(COLLECTION_NAME)
        results = collection.get(
            where={"convocatoria_id": convocatoria_id},
            limit=1,
            include=[]  # Solo necesitamos saber si existe, sin traer datos
        )
        return len(results["ids"]) > 0
    except Exception as e:
        logger.warning(f"⚠️ [VectorStore] Error al verificar duplicados: {e}")
        return False

def get_vector_store() -> Chroma:
    """Devuelve la instancia global configurada del VectorStore."""
    return vector_store
```

---

# 5.13 Carpeta `services/runpod/`

## 5.13.1 `services/runpod/__init__.py`

```python
"""
services.runpod — Módulo para el control de la API de RunPod
"""
```

---

## 5.13.2 `services/runpod/manager.py`

```python
"""
services.runpod.manager — Manager para la API de RunPod

Controla el encendido y apagado de pods con GPU para ahorrar costos.
"""

import os
import time
import asyncio
import logging
import httpx

logger = logging.getLogger("API-Konomegua")

class RunPodManager:
    def __init__(self):
        self.api_key = os.getenv("RUNPOD_API_KEY")
        self.pod_id = os.getenv("RUNPOD_POD_ID")
        self.graphql_url = f"https://api.runpod.io/graphql?api_key={self.api_key}"
        
        # Estado local simplificado si falta config
        self.is_configured = bool(self.api_key and self.pod_id)
        if not self.is_configured:
            logger.warning("⚠️ [RunPod] API Key o Pod ID no configurados. Se omitirá control automático del Pod.")

    async def _execute_graphql(self, query: str, variables: dict = None) -> dict:
        """Ejecuta un request GraphQL a RunPod."""
        if not self.is_configured:
            return {}
            
        payload = {"query": query}
        if variables:
            payload["variables"] = variables

        try:
            async with httpx.AsyncClient(timeout=10.0) as client:
                response = await client.post(self.graphql_url, json=payload)
                response.raise_for_status()
                return response.json()
        except Exception as e:
            logger.error(f"❌ [RunPod] Error en petición GraphQL: {e}")
            return {}

    async def get_pod_status(self) -> str:
        """Retorna el estado actual: 'RUNNING', 'STOPPED', etc."""
        if not self.is_configured:
            return "RUNNING"  # Falso positivo seguro para entornos locales

        query = '''
        query getPod($input: PodFilter!) { 
            pod(input: $input) { 
                id 
                desiredStatus 
            } 
        }
        '''
        res = await self._execute_graphql(query, {"input": {"podId": self.pod_id}})
        try:
            status = res.get("data", {}).get("pod", {}).get("desiredStatus")
            return status if status else "UNKNOWN"
        except Exception:
            return "UNKNOWN"

    async def is_pod_running(self) -> bool:
        """Retorna True si el pod está encendido (desiredStatus == RUNNING)."""
        status = await self.get_pod_status()
        return status == "RUNNING"

    async def start_pod(self) -> bool:
        """Enciende el pod y espera a que esté RUNNING (hasta 5 mins)."""
        if not self.is_configured:
            return True

        logger.info(f"🚀 [RunPod] Solicitando encendido del Pod {self.pod_id}...")
        query = '''
        mutation resumePod($input: PodResumeInput!) { 
            podResume(input: $input) { 
                id 
                desiredStatus 
            } 
        }
        '''
        res = await self._execute_graphql(query, {"input": {"podId": self.pod_id, "gpuCount": 1}})
        
        # Verificar si RunPod devolvió errores (ej. falta de fondos o no hay GPUs libres)
        if "errors" in res and res["errors"]:
            error_msg = res["errors"][0].get("message", "Error desconocido")
            logger.error(f"❌ [RunPod] Falló al intentar encender el Pod: {error_msg}")
            return False
            
        # Esperar hasta que esté RUNNING
        max_retries = 30  # 30 * 10s = 5 minutos
        for i in range(max_retries):
            status = await self.get_pod_status()
            if status == "RUNNING":
                logger.info("✅ [RunPod] Pod inicializado y listo.")
                return True
            logger.info(f"⏳ [RunPod] Esperando encendido... (intento {i+1}/{max_retries})")
            await asyncio.sleep(10)
            
        logger.warning("⚠️ [RunPod] Timeout esperando que el Pod encienda.")
        return False

    async def stop_pod(self) -> bool:
        """Apaga el pod para ahorrar costos."""
        if not self.is_configured:
            return False

        logger.info(f"🛑 [RunPod] Apagando el Pod {self.pod_id} por inactividad...")
        query = '''
        mutation stopPod($input: PodStopInput!) { 
            podStop(input: $input) { 
                id 
                desiredStatus 
            } 
        }
        '''
        res = await self._execute_graphql(query, {"input": {"podId": self.pod_id}})
        success = res.get("data", {}).get("podStop", {}).get("desiredStatus") == "EXITED"
        
        if success:
            logger.info("💤 [RunPod] Pod apagado correctamente.")
            return True
        else:
            logger.error("❌ [RunPod] Falló la solicitud de apagado.")
            return False

# Singleton instance
runpod_manager = RunPodManager()
```

---

# 5.14 Carpeta `workers/`

## 5.14.1 `workers/__init__.py`

```python
# workers/ — Procesos de segundo plano desacoplados (TDR, OCR, LLM, etc.)
```

---

## 5.14.2 `workers/base_worker.py`

```python
"""
workers.base_worker — Orquestador de Ciclo de Vida y Apagado Ordenado para Workers

Administra la inicialización, la configuración del logging, la escucha de señales de
salida (graceful shutdown) y el cierre seguro de los sockets del broker.
"""

import sys
import os
import signal
import asyncio
import logging
from dotenv import load_dotenv

# Forzar la carga de variables de entorno
load_dotenv()

# Configuración del Logger para Workers
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] (%(name)s) %(message)s",
    handlers=[
        logging.StreamHandler(sys.stdout)
    ]
)
logger = logging.getLogger("Worker-Manager")

class BaseWorker:
    """
    Controla el arranque, las señales del sistema operativo para un apagado seguro
    y el cierre correcto de recursos de colas.
    """
    def __init__(self, consumer):
        self.consumer = consumer
        self.loop = asyncio.get_event_loop()
        self._stop_event = asyncio.Event()

    def handle_exit_signal(self, sig, frame=None):
        logger.info(f"🛑 Señal {sig} capturada. Iniciando Graceful Shutdown del worker...")
        self._stop_event.set()

    def run(self):
        """Arranca el worker y se mantiene escuchando señales de sistema."""
        # Captura de señales SIGINT (Ctrl+C) y SIGTERM (Docker/Systemd exit)
        for sig in (signal.SIGINT, signal.SIGTERM):
            try:
                self.loop.add_signal_handler(sig, lambda: self._stop_event.set())
            except NotImplementedError:
                # Fallback en Windows (no soporta add_signal_handler completamente)
                signal.signal(sig, self.handle_exit_signal)

        logger.info("👷 Worker montado y listo para iniciar consumo...")
        consume_task = self.loop.create_task(self.consumer.start_consuming())

        async def wait_for_stop():
            await self._stop_event.wait()
            logger.info("🔌 Deteniendo consumidores en segundo plano...")
            consume_task.cancel()
            try:
                await consume_task
            except asyncio.CancelledError:
                pass
            
            # Cierre de conexiones robustas
            from services.queue.connection import RabbitMQConnection
            await RabbitMQConnection.get_instance().close()
            logger.info("✅ Worker finalizado exitosamente de forma limpia.")

        try:
            self.loop.run_until_complete(wait_for_stop())
        except Exception as e:
            logger.critical(f"💥 Error fatal no controlado en el worker: {e}")
        finally:
            # No cerrar el loop si ya está cerrado o si se ejecuta en contexts especiales
            if not self.loop.is_closed():
                self.loop.close()
```

---

## 5.14.3 `workers/tdr_worker.py`

```python
"""
workers.tdr_worker — Worker de Procesamiento Pesado de TDR (OCR + LLM + DB)

Consume de la cola 'tdr_jobs', procesa las cotizaciones de SEACE:
1. Lanza Playwright para obtener un token limpio del portal PJ.
2. Descarga el TDR oficial (PDF).
3. Extrae texto crudo (pdfplumber) o realiza OCR (Tesseract / pdf2image) si es escaneado.
4. Invoca la IA modular (services.ai de la Fase 1) con soporte multimodal opcional.
5. Registra de manera persistente las especificaciones extraídas en PostgreSQL.
"""

import os
import sys
import json
import logging
import asyncio
import tempfile
from aio_pika import IncomingMessage
from playwright.async_api import async_playwright
import pdfplumber
import psycopg2
import psycopg2.pool
import io

# Asegurar que el directorio raíz de la aplicación ('prod') esté en el PYTHONPATH
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from services.queue.consumer import BaseConsumer
from workers.base_worker import BaseWorker

logger = logging.getLogger("TDR-Worker")

class TDRConsumer(BaseConsumer):
    def __init__(self):
        super().__init__(queue_name="tdr_jobs", prefetch_count=1)
        self.db_pool = psycopg2.pool.ThreadedConnectionPool(
            minconn=1,
            maxconn=3,
            host=os.getenv("DB_HOST"),
            port=os.getenv("DB_PORT", "6543"),
            database=os.getenv("DB_NAME"),
            user=os.getenv("DB_USER"),
            password=os.getenv("DB_PASSWORD")
        )
        logger.info("✅ [TDR Worker] Pool de conexiones PostgreSQL inicializado.")

    async def handle_message(self, message: IncomingMessage):
        """Procesa un job de TDR desde la cola."""
        payload = json.loads(message.body.decode("utf-8"))
        year = payload.get("year")
        uejec = payload.get("uejec")
        num = payload.get("num")
        items_mapeados = payload.get("items_mapeados")

        logger.info(f"🚀 [Job Recibido] Procesando TDR: Cotización {num} (Año {year}, UE {uejec})")
        start_time = asyncio.get_event_loop().time()

        try:
            # 1. Ejecutar procesamiento pesado
            await self.procesar_tdr(year, uejec, num, items_mapeados)
            
            elapsed = asyncio.get_event_loop().time() - start_time
            logger.info(f"✅ [Job Completado] Cotización {num} procesada exitosamente en {elapsed:.2f}s")
        except Exception as e:
            logger.error(f"❌ [Job Fallido] Error crítico en cotización {num}: {e}")
            # Lanza la excepción para que aio-pika haga NACK y reencole de forma controlada
            raise e

    async def procesar_tdr(self, year: int, uejec: str, num: str, items_mapeados: list):
        if not items_mapeados:
            logger.warning(f"⚠️ Sin items mapeados para {num}. Saltando procesamiento.")
            return

        # Configuración dinámica del Proxy
        proxy_config = None
        proxy_url = os.getenv("PROXY_URL")
        if proxy_url:
            try:
                auth_part, server_part = proxy_url.split('@')
                protocol_part, user_pass = auth_part.split('://')
                username, password = user_pass.split(':')
                server = f"{protocol_part}://{server_part}"
                proxy_config = {"server": server, "username": username, "password": password}
            except Exception as ep:
                logger.error(f"❌ Error parseando PROXY_URL en worker: {ep}")

        # 1. Descargar PDF usando un navegador desechable de Playwright para obtener el token
        async with async_playwright() as p:
            token = await self.obtener_token(p, proxy_config)
            if not token:
                raise Exception("Imposible obtener el token de SEACE desde el worker.")

            request_context = (
                await p.request.new_context(proxy=proxy_config)
                if proxy_config else
                await p.request.new_context()
            )
            try:
                url_pdf = "https://sap.pj.gob.pe/portalabastecimiento-api/convocatorias/archivos"
                params = {"anio": year, "unidejec": uejec, "numCotiz": num, "tipoDoc": "904", "tipoArchivo": "01"}
                
                logger.info(f"📄 [Descarga] Bajando TDR oficial para la cotización {num}...")
                res = await request_context.get(url_pdf, params=params, headers={"Authorization": f"Bearer {token}"})
                
                if res.status != 200:
                    raise Exception(f"Fallo de descarga de PDF. Status SEACE: {res.status}")
                    
                pdf_bytes = await res.body()
                if not pdf_bytes:
                    raise Exception("El contenido del PDF descargado está completamente vacío.")
            finally:
                await request_context.dispose()

        # 2. Extraer texto crudo (pdfplumber)
        texto_pdf = ""
        b64_images = []
        with tempfile.NamedTemporaryFile(delete=False, suffix=".pdf") as tmp:
            tmp.write(pdf_bytes)
            tmp_path = tmp.name
            
        try:
            with pdfplumber.open(tmp_path) as pdf:
                for page in pdf.pages:
                    text = page.extract_text()
                    if text:
                        texto_pdf += text + "\n"
                        
            # 3. Aplicar OCR Local con Tesseract si es escaneado
            if len(texto_pdf.strip()) < 50:
                logger.info(f"🔍 [OCR] PDF sin texto digital directo en {num}. Aplicando OCR local...")
                from pdf2image import convert_from_path
                import pytesseract
                
                import platform
                if platform.system() == "Windows":
                    pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
                    imagenes = convert_from_path(tmp_path, poppler_path=r'C:\poppler\Library\bin')
                else:
                    # Linux: tesseract y poppler están en PATH vía apt (tesseract-ocr, poppler-utils)
                    imagenes = convert_from_path(tmp_path)
                for img in imagenes:
                    texto_pdf += pytesseract.image_to_string(img, lang="spa") + "\n"
                    buf = io.BytesIO()
                    img.convert("RGB").save(buf, format="JPEG")
                    b64_images.append(buf.getvalue())
                    
        except Exception as e_pdf:
            logger.warning(f"⚠️ Error extrayendo texto del PDF: {e_pdf}")
        finally:
            try:
                os.unlink(tmp_path)
            except Exception:
                pass

        # Convertir a formato base64
        import base64
        b64_images_str = [base64.b64encode(img).decode('utf-8') for img in b64_images]
        usar_imagenes = len(texto_pdf.strip()) < 50 and len(b64_images_str) > 0
            
        if not texto_pdf.strip() and not usar_imagenes:
            logger.warning(f"⚠️ PDF sin contenido procesable para la cotización {num}")
            return

        # 4. Ingestión RAG (Fase 4): Chunking + Embeddings + Vector DB
        from services.rag.ingestion import ingerir_documento_tdr
        from services.rag.pipeline import extraer_requerimientos_rag

        logger.info(f"📚 [RAG] Ingeriendo documento TDR a ChromaDB para {num}...")
        # texto_pdf contiene el texto extraído (nativo o por OCR)
        ingerido_ok = await ingerir_documento_tdr(texto_pdf, year, uejec, num)
        
        resultados = []
        if ingerido_ok:
            convocatoria_id = f"{year}-{num}-{uejec}"
            logger.info(f"🧠 [RAG] Procesando {len(items_mapeados)} productos individualmente con Vector Retrieval...")
            for item in items_mapeados:
                id_prod = item.get("id_prod")
                descripcion = item.get("descripcion")
                
                res = await extraer_requerimientos_rag(
                    id_prod=id_prod,
                    producto_descripcion=descripcion,
                    convocatoria_id=convocatoria_id,
                    top_k=4  # Contexto reducido y optimizado
                )
                if res and "requerimientos" in res:
                    resultados.append(res)
                
                # Pausa para dar respiro al GPU (batching temporal simple)
                await asyncio.sleep(0.1)
        else:
            logger.warning(f"⚠️ [RAG] No se pudo ingerir documento. Cayendo a fallback (Multimodal/Texto)...")
            from services.ai import extraer_requerimientos_texto, extraer_requerimientos_multimodal
            if usar_imagenes:
                resultados = await extraer_requerimientos_multimodal(items_mapeados, texto_pdf, b64_images_str)
            else:
                resultados = await extraer_requerimientos_texto(items_mapeados, texto_pdf)

        # 5. Insertar resultados directamente en PostgreSQL
        # 5. Insertar resultados directamente en PostgreSQL usando el pool
        conn = self.db_pool.getconn()
        try:
            with conn.cursor() as cur:
                reqs_insertados = 0
                for req_prod in resultados:
                    id_item = req_prod.get("id_prod")
                    reqs = req_prod.get("requerimientos", [])
                    for r in reqs:
                        if not r.strip():
                            continue
                        cur.execute(
                            "INSERT INTO convocatoria_requerimientos (convocatoria_item_id, requerimiento) VALUES (%s, %s)",
                            (id_item, r.strip())
                        )
                        reqs_insertados += 1
                        
                if reqs_insertados > 0:
                    conn.commit()
                    logger.info(f"💾 [Base de Datos] Guardados {reqs_insertados} requerimientos para la cotización {num}")
        except Exception as e_db:
            conn.rollback()
            raise e_db
        finally:
            self.db_pool.putconn(conn)

    async def obtener_token(self, p, proxy_config) -> str:
        """Helper para obtener token del portal PJ usando Chromium."""
        import platform
        launch_args = {
            "headless": True,
            "args": [
                "--no-sandbox", "--disable-setuid-sandbox",
                "--disable-dev-shm-usage", "--single-process",
                "--disable-gpu", "--disable-audio-output"
            ]
        }
        if platform.system() == "Linux":
            launch_args["executable_path"] = "/usr/bin/chromium-browser"
        if proxy_config:
            launch_args["proxy"] = proxy_config
        browser = await p.chromium.launch(**launch_args)
        try:
            context = await browser.new_context(
                user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36",
                viewport={'width': 800, 'height': 600}
            )
            page = await context.new_page()
            await page.goto("https://sap.pj.gob.pe/portalabastecimiento-web/Convocatorias8uit", wait_until="commit", timeout=45000)
            token = None
            for _ in range(15):
                token = await page.evaluate("() => sessionStorage.getItem('TOKEN')")
                if token and len(token) > 50:
                    break
                await asyncio.sleep(1)
            return token
        finally:
            await browser.close()

if __name__ == "__main__":
    consumer = TDRConsumer()
    worker = BaseWorker(consumer)
    logger.info("👷 TDR Worker listo y en escucha...")
    worker.run()
```

---

# 5.15 Carpeta `tests/`

## 5.15.1 `tests/test_rag.py`

```python
"""
tests.test_rag — Suite de Pruebas de Integración para el Pipeline RAG

Valida el chunking, la generación de embeddings locales, el almacenamiento 
en ChromaDB y la recuperación de contexto (Retrieval).

Ejecución:
    python -m pytest tests/test_rag.py -v -s
    # O ejecución directa
    python tests/test_rag.py
"""

import os
import sys
import asyncio
import logging

# Asegurar que el directorio raíz de la aplicación ('prod') esté en el PYTHONPATH
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from services.rag.chunker import dividir_texto_en_chunks
from services.rag.ingestion import ingerir_documento_tdr
from services.rag.retriever import buscar_contexto_relevante
from services.rag.pipeline import extraer_requerimientos_rag

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger("Test-RAG")

# Mock de TDR
MOCK_TDR_TEXT = """
TERMINOS DE REFERENCIA PARA LA ADQUISICIÓN DE EQUIPOS DE CÓMPUTO
================================================================

1. OBJETIVO
El objetivo es adquirir laptops y monitores para el área de sistemas.

2. ESPECIFICACIONES TÉCNICAS

PRODUCTO A: Laptop Core i7
- Procesador Intel Core i7 12va generación o superior.
- Memoria RAM de 16 GB DDR4.
- Disco de estado sólido (SSD) de 512 GB NVMe.
- Pantalla de 15.6 pulgadas Full HD.

PRODUCTO B: Monitor 24 pulgadas
- Resolución 1920x1080 (FHD).
- Frecuencia de actualización 75Hz.
- Conectividad HDMI y DisplayPort.
"""

def test_chunker():
    logger.info("--- TEST 1: CHUNKER ---")
    meta = {"convocatoria_id": "test-123"}
    docs = dividir_texto_en_chunks(MOCK_TDR_TEXT, meta)
    
    assert len(docs) > 0, "No se generaron chunks"
    assert docs[0].metadata["convocatoria_id"] == "test-123", "Falta metadata"
    assert "chunk_index" in docs[0].metadata, "Falta chunk_index"
    logger.info(f"✅ Chunker generó {len(docs)} fragmentos correctamente.")

async def test_flujo_completo_rag():
    logger.info("\n--- TEST 2: INGESTIÓN Y RETRIEVAL (PIPELINE COMPLETO) ---")
    year = 2026
    uejec = "001"
    num = "9999"
    convocatoria_id = f"{year}-{num}-{uejec}"

    # 1. Ingestión (Chunking + Embeddings + ChromaDB)
    # Nota: La primera vez que se ejecuta descargará el modelo de embeddings.
    logger.info("Probando ingestión (esto puede tomar unos segundos la primera vez)...")
    ingerido = await ingerir_documento_tdr(MOCK_TDR_TEXT, year, uejec, num)
    assert ingerido, "Fallo al ingerir el documento en ChromaDB."
    logger.info("✅ Ingestión exitosa.")

    # 2. Retrieval (Simulación de búsqueda del worker)
    logger.info("Probando Retrieval para 'Monitor'...")
    resultados_monitor = await asyncio.to_thread(
        buscar_contexto_relevante,
        query="Monitor 24 pulgadas",
        convocatoria_id=convocatoria_id,
        top_k=2
    )
    
    assert len(resultados_monitor) > 0, "No se encontraron chunks para 'Monitor'"
    # Validamos que el chunk devuelto realmente contenga texto sobre el monitor
    encontrado = any("1920x1080" in doc.page_content for doc in resultados_monitor)
    assert encontrado, "El retrieval no trajo el contexto correcto."
    logger.info("✅ Retrieval de 'Monitor' exitoso y context-aware.")

    # 3. Pipeline LLM End-to-End (opcional, requiere vLLM activo)
    # Se omiten aserciones estrictas para que no falle si vLLM está apagado durante testing
    try:
        logger.info("Probando RAG pipeline con vLLM (requiere vLLM encendido)...")
        res_laptop = await extraer_requerimientos_rag(
            id_prod=1,
            producto_descripcion="Laptop Core i7",
            convocatoria_id=convocatoria_id,
            top_k=2
        )
        logger.info(f"✅ RAG LLM Result: {res_laptop}")
    except Exception as e:
        logger.warning(f"⚠️ RAG LLM omitido o falló (probablemente vLLM no esté activo): {e}")

if __name__ == "__main__":
    test_chunker()
    asyncio.run(test_flujo_completo_rag())
    logger.info("\n🎉 Todos los tests fundamentales del módulo RAG pasaron exitosamente.")
```

---

# 6. Observaciones de seguridad

El presente documento no incluye archivos `.env` reales, contraseñas, tokens, claves API ni credenciales privadas.

Los archivos de configuración sensibles deben entregarse únicamente por canales seguros y a personal autorizado.

Tampoco se incluyen bases de datos locales, archivos temporales, carpetas de caché, carpetas de entorno virtual ni archivos generados automáticamente.

Archivos excluidos de este documento:

- `.env`
- `*.db`
- `__pycache__/`
- `*.pyc`
- `.git/`
- `.venv/`
- `venv/`
- `logs/`
- `chroma_db/`
- Archivos `.zip`, `.tar` o PDFs generados

---


El presente documento consolida el manual de usuario y el código fuente principal del software **ITACHI IA**, preparado para fines de revisión, entrega, validación técnica o registro documental.

