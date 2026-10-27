# Monitoreo de cadena de frío para comercios chicos

Sistema IoT que mide la temperatura de una heladera de forma continua y avisa al dueño en el momento en que se pasa del umbral.

> Los campos marcados con **[COMPLETAR]** dependen de decisiones del equipo.

---

## 1. Problema a resolver

**El problema:** si la heladera de un comercio falla de noche, la mercadería se echa a perder y nadie se entera hasta la mañana siguiente.

**Quién lo tiene:** comercios chicos que dependen de la refrigeración, como una carnicería, un kiosco o una farmacia. No tienen sistemas de monitoreo profesionales y nadie está presente fuera del horario de atención.

**Por qué una solución IoT lo resuelve:**
- Un sensor mide la temperatura todo el tiempo, sin que nadie tenga que revisar.
- El aviso llega en el momento en que se supera el umbral, no al día siguiente.
- Se guarda un historial de temperaturas, útil para saber cuánto duró la falla.
- Es barato y se puede instalar sin obras.

---

## 2. Arquitectura

```
ESP32 (sensor)  →  MQTT  →  Node-RED  →  Base de datos  →  n8n  →  Alerta
```

| Etapa | Herramienta | Función |
|---|---|---|
| Dispositivo | ESP32 + sensor de temperatura **[COMPLETAR: modelo]** | Mide y publica la temperatura |
| Protocolo | MQTT (broker **[COMPLETAR: Mosquitto u otro]**) | Transporta las lecturas |
| Procesamiento | Node-RED | Recibe los mensajes, los procesa y los guarda |
| Base de datos | **[COMPLETAR: PostgreSQL, MySQL, etc.]** | Guarda el historial de lecturas |
| Automatización | n8n | Detecta que se pasó del umbral y envía la alerta |
| Alerta | **[COMPLETAR: Telegram, WhatsApp, correo]** | Avisa al dueño |

---

## 3. Herramientas usadas

- **Automatización:** Node-RED y n8n.
- **Base de datos:** **[COMPLETAR]**.
- **Protocolo de comunicación:** MQTT.
- **Dispositivo:** **[COMPLETAR: hardware real con ESP32 / simulado con Wokwi / script en Python]**.
- **Visualización (opcional):** **[COMPLETAR: dashboard de Node-RED, Grafana o ninguna]**.

---

## 4. Dispositivos y datos

**Tema MQTT:** `[COMPLETAR]` (por ejemplo, `comercio/heladera1/temperatura`)

**Formato del mensaje (propuesta):**

```json
{
  "dispositivo": "heladera1",
  "temperatura": 4.2,
  "timestamp": "2026-10-22T03:15:00Z"
}
```

**Frecuencia de medición:** **[COMPLETAR]**

---

## 5. Base de datos

**Tabla de lecturas (propuesta):**

| Campo | Tipo | Descripción |
|---|---|---|
| id | entero, autoincremental | Identificador |
| dispositivo | texto | Nombre del sensor o heladera |
| temperatura | decimal | Temperatura en °C |
| fecha_hora | fecha y hora | Momento de la lectura |

---

## 6. Lógica de alertas

- **Umbral:** **[COMPLETAR]** °C. Debe ser configurable según el tipo de comercio. Conviene verificar los rangos recomendados para cada producto (alimentos, medicamentos) en una fuente oficial.
- **Condición:** la alerta se envía cuando la temperatura supera el umbral durante **[COMPLETAR]** minutos seguidos, para evitar falsas alarmas cuando se abre la puerta.
- **Mensaje de alerta (ejemplo):** "Heladera 1: 9 °C desde hace 15 minutos. Revisar."
- **Evitar repeticiones:** n8n no debería reenviar la misma alerta en cada lectura mientras dure la falla.

---

## 7. Cómo ejecutarlo

1. Levantar el broker MQTT y la base de datos. **[COMPLETAR]**
2. Importar el flujo de Node-RED (`flows/nodered.json`). **[COMPLETAR]**
3. Importar el flujo de n8n (`flows/n8n.json`). **[COMPLETAR]**
4. Cargar el código en el ESP32 o iniciar la simulación. **[COMPLETAR]**
5. Configurar las credenciales (broker, base de datos, canal de alerta) en un archivo de variables de entorno. No subir contraseñas al repositorio.

---

## 8. Estructura del repositorio (propuesta)

```
├── README.md
├── firmware/        # código del ESP32 o simulación
├── flows/           # flujos exportados de Node-RED y n8n
├── database/        # script de creación de tablas
└── docs/            # capturas y diagrama
```

---

## 9. Posibles mejoras

- **Alerta por silencio:** si dejan de llegar lecturas, avisar que el sistema no responde.
- **Respaldo de energía:** batería para el ESP32 y respaldo para el router, o un módulo de datos móviles, para que el sistema siga avisando durante un corte de luz.
- **Dashboard** con el historial de temperaturas.
- **Varias heladeras** en un mismo comercio.

---

## 10. Equipo

Ariel Zeñuka
Armando Brunner
Luciana Monllor
