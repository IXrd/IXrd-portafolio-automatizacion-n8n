# 🤖 AI Automotive Sales Agent & Analytics Suite

> **Sistema inteligente de captación de leads, atención al cliente y análisis de ventas para concesionarios automotrices.**

Este proyecto implementa un ecosistema de automatización con **n8n** que transforma la interacción con el cliente en WhatsApp. Utiliza Inteligencia Artificial Generativa (LLMs) para calificar prospectos ("Lead Scoring"), gestionar inventario en tiempo real y coordinar citas, todo conectado a un Dashboard de control.

![Dashboard UI](assets/dashboard.png)
*(Vista del Dashboard de control generado por el sistema)*

---

## 🚀 Características Principales

### 1. Agente de Ventas Omnicanal (WhatsApp)
El núcleo del sistema (`agente-ventas-whatsapp.json`) gestiona la interacción directa con el cliente:
* **Procesamiento Multimedia:** Capaz de escuchar notas de voz (Transcribe con OpenAI Whisper) y "ver" imágenes enviadas por el usuario (OpenAI Vision) para entender qué vehículo buscan.
* **Gestión de Estado (Redis):** Control de concurrencia y prevención de mensajes duplicados para asegurar una respuesta fluida.
* **Ruteo Inteligente:** Un clasificador de intenciones decide si el usuario quiere **Comprar** (deriva al Agente de Ventas) o tiene una duda general (deriva al Agente de FAQ).
* **Inventario en Tiempo Real:** Conexión con **Airtable** para consultar stock, precios y enviar fotos de vehículos específicos sin alucinaciones.

### 2. RAG (Retrieval-Augmented Generation) Institucional
* Utiliza **Supabase (Vector Store)** para almacenar y recuperar información institucional (financiamiento, ubicación, garantías).
* El bot responde preguntas complejas basándose estrictamente en la documentación del dealer, reduciendo el riesgo de respuestas incorrectas.

### 3. Analytics & Lead Scoring
El sistema no solo chatea, también analiza (`dashboard-data.json` y `dashboard-agente.json`):
* **KPIs en Vivo:** Calcula tráfico total, usuarios únicos y leads calificados ("Hot Leads").
* **Análisis de Sentimiento:** Evalúa las conversaciones para determinar la intención de compra real.
* **Reportes Visuales:** Genera datos estructurados para visualizar tendencias de ventas y financiamiento.

---

## 🛠️ Stack Tecnológico

* **Orquestación:** [n8n](https://n8n.io/)
* **LLMs & AI:** OpenAI (GPT-4o, GPT-4o-mini, Whisper, Vision).
* **Base de Datos & Vectores:** Supabase (PostgreSQL + pgvector).
* **Base de Datos (Cache):** Redis.
* **CMS / Inventario:** Airtable.
* **Mensajería:** Evolution API (WhatsApp Gateway).
* **Integración Humana:** Chatwoot (para derivación a agentes humanos si es necesario).

---

## 🧠 Arquitectura de los Flujos

### 🚗 Flujo Principal: Agente de Ventas
![Flujo Principal n8n](assets/flujoprincipal.png)


**Lógica del flujo:**
1.  **Ingesta:** Recibe el mensaje (Texto, Audio o Imagen) vía Webhook.
2.  **Validación:** Verifica en Redis si el mensaje es duplicado o si el usuario está en "cooldown".
3.  **Clasificación:** Un nodo de IA determina la intención (`Ventas`, `FAQ`, `Agendar Cita`).
4.  **Ejecución:**
    * *Si es Venta:* Consulta Airtable buscando marca/modelo y devuelve ficha técnica con fotos.
    * *Si es FAQ:* Consulta Supabase Vector Store para políticas del dealer.
    * *Si es Cita:* Coordina horario y guarda el lead en el CRM (Airtable).

### 📊 Flujo de Analytics (Dashboard)
![Backend Dashboard](assets/BackendDashboard.png)

**Lógica del flujo:**
1.  Recopila historiales de chat de Supabase.
2.  Procesa los datos para distinguir entre "Leads de Financiamiento" vs "Venta Directa".
3.  Calcula variaciones porcentuales (Mes actual vs Mes anterior).
4.  Entrega un JSON estructurado listo para ser consumido por el Frontend del Dashboard.

---

## 📦 Instalación y Despliegue

Este proyecto requiere una instancia de n8n (self-hosted o cloud).

1.  **Requisitos Previos:**
    * Cuenta de OpenAI (API Key).
    * Proyecto en Supabase configurado.
    * Base de Airtable con tablas de `Vehículos` y `Clientes`.
    * Instancia de Redis.

2.  **Importar Flujos:**
    * Importar los archivos `.json` ubicados en la carpeta `/N8N GITHUB REPO` dentro de n8n.

3.  **Configurar Credenciales:**
    * Configurar los nodos de credenciales para OpenAI, Supabase, Redis y Airtable dentro del editor de n8n.

---

## 📬 Contacto

Brian Melo
Especialista en IA & Automatización de Procesos
brianmelo1228@gmail.com
829-748-5994 No. de Whatsapp

> *Proyecto desarrollado como demostración de capacidades para implementación de IA en el sector automotriz.*

