# Estado del Proyecto Jarvis-Chetumal

## Servicios Corriendo (Docker Swarm + standalone)

### Stack: jarvis (docker stack)
- `jarvis_ollama` — Ollama con Llama 3.2:3b, puerto interno 11434, nodo daniel-B450M-H
- `jarvis_webui` — Open WebUI puerto 3000, Gemini API conectada, sistema prompt Jarvis configurado

### Stack: services (docker stack)
- `services_postgres` — PostgreSQL 16, puerto 5432, usuario: jarvis, db: n8n + evolution
- `services_n8n` — n8n puerto 5678, zona horaria America/Cancun
- `services_qdrant` — Qdrant (memoria vectorial), red interna
- `services_redis` — Redis 7, puerto 6379
- `services_searxng` — SearXNG puerto 8888 (configurar en Open WebUI)

### Standalone (docker run, NO en Swarm)
- `evolution-api` — Evolution API v2.1.1 puerto 8080, host network
  - PROBLEMA: Baileys no genera QR, se desconecta de WhatsApp en loop
  - Causa probable: versión de Baileys desactualizada (2,3000,1015901307)
  - Pendiente: probar WAHA como alternativa

## Credenciales del Sistema
- Evolution API key: `jarvis-evolution-2026`
- Postgres user/pass: `jarvis` / `jarvis_db_2026`
- n8n encryption key: `56f196edb63cd14f729de6a09d3899325a1513978455ef3d`
- GitHub repo: https://github.com/ing-daniel-perez/jarvis-chetumal

## URLs de Acceso Local
- Jarvis (Open WebUI): http://localhost:3000
- n8n: http://localhost:5678
- SearXNG: http://localhost:8888
- Evolution API: http://localhost:8080
- Evolution Manager: http://localhost:8080/manager

## Pendiente (en orden de prioridad)

### 1. WhatsApp (BLOQUEADO — Evolution API no funciona)
- [ ] Probar WAHA como alternativa: `docker run -d --name waha --network host -e WHATSAPP_DEFAULT_ENGINE=WEBJS devlikeapro/waha`
- [ ] Si WAHA funciona: crear instancia y escanear QR
- [ ] Configurar webhook de WhatsApp → n8n
- [ ] Flujo n8n: recibir mensaje → detectar tipo → procesar

### 2. Flujos n8n (listo para construir)
- [ ] Flujo de flyers: detectar tipo (resultado/partido/tabla/goleadores) con Ollama
- [ ] Extraer datos de SACAF automáticamente
- [ ] Integración con Canva API para generar flyers
- [ ] Flujo de tareas escolares: detectar fecha + asignatura + descripción
- [ ] Lista de clientes: nuevo número → agregar automáticamente

### 3. Voz (pendiente)
- [ ] Faster-Whisper para STT (español)
- [ ] Piper TTS con voz española latina para respuestas de Jarvis

### 4. Acceso externo (pendiente)
- [ ] Cloudflare Tunnel para usar Jarvis desde cualquier lugar

### 5. Configuración pendiente en Open WebUI
- [ ] Verificar que SearXNG está conectado (URL: http://searxng:8888)
- [ ] Confirmar que modelos Gemini aparecen en selector

## Arquitectura de Datos
- Gemini API (Google AI Studio free): preguntas generales, código, aprendizaje
- Ollama local (llama3.2:3b): datos privados de clientes, WhatsApp, proyectos sensibles
- n8n + Ollama: automatización de WhatsApp para flyers

## Contexto del Usuario
- Hace flyers de equipos de fútbol (resultado, próximo partido, tabla, goleadores)
- Flujo actual: WhatsApp → SACAF (SaaS fútbol) → Excel con macros → Canva por lote
- También gestiona tareas escolares via WhatsApp
- PC Ryzen (daniel-B450M-H): 16GB RAM, 20 hilos, sin GPU — nodo principal
- Laptop Lenovo (daniel-Lenovo-ideapad-320-15ISK): 8GB RAM — gateway WiFi→Ethernet
- NFS: /home/daniel/ClusterStorage (4TB en Laptop, montado en PC)
