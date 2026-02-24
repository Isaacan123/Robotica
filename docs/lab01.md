# 📚 Work 4: FreeRTOS Labs (Tasks, Queues & Mutex)

> Documentación de las prácticas de sistemas operativos en tiempo real (RTOS) utilizando ESP32, abarcando planificación de tareas, colas de mensajes y manejo de recursos compartidos.

---

## 1) Resumen

- **Nombre del proyecto:** FreeRTOS Labs 01, 02 y 03
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica
- **Fecha:** 19/02/2026
- **Descripción breve:** Implementación y análisis de sistemas multitarea en ESP32 utilizando FreeRTOS. Se exploran las prioridades de las tareas, la inanición (starvation), el modelo productor/consumidor mediante colas (`Queues`) y la resolución de condiciones de carrera con `Mutex`.

!!! tip "Consejo"
    Recuerda que en FreeRTOS para ESP-IDF, el tamaño del *stack* (`stack size`) al crear una tarea con `xTaskCreate` se define en **Bytes**, no en *Words* como en Vanilla FreeRTOS.

---

## 2) Objetivos

- **General:** Understand FreeRTOS task creation, scheduling, and inter-task communication.
- **Específicos:**
  - Implementar el control de GPIO para el parpadeo de un LED en ESP32.
  - Gestionar retardos de tiempo usando la función `vTaskDelay`.
  - Configurar correctamente las prioridades y tamaños de *stack* de las tareas.
  - Analizar el comportamiento del sistema ante cambios de prioridad e inanición (starvation).
  - Implementar colas (`Queues`) para el envío y recepción de estructuras de datos.
  - Identificar condiciones de carrera (Race conditions) y proteger recursos compartidos.

---

## 3) Alcance y Exclusiones

- **Incluye:** Código de prueba para 3 laboratorios experimentales y resolución de cuestionarios teóricos sobre el comportamiento del RTOS.
- **No incluye:** Configuración desde cero del entorno ESP-IDF ni el esquemático de conexión del botón físico del Lab 3.

---

## 4) Requisitos

**Software**
- Entorno de desarrollo ESP-IDF o Arduino IDE con soporte para ESP32.
- Librerías base de FreeRTOS (`freertos/FreeRTOS.h`, `freertos/task.h`, `freertos/queue.h`).
- Librerías de ESP32 (`driver/gpio.h`, `esp_log.h`).

**Materiales**
- No materials required (Se puede simular o ejecutar directamente en la placa ESP32 base).

---

## 5) Procedimiento y Análisis (Exercises)

### Lab 01: Tasks & Priorities
- **Priority experiment:** Se cambió la prioridad de `hello_task` de 5 a 2.
  - *¿Cambia el comportamiento?* Sí. Como bajamos la prioridad, la tarea de parpadeo (blink) se ejecuta antes o interrumpe a la otra tarea, dándole preferencia de CPU a la tarea con el número mayor.
- **Starvation demo:** Se removió temporalmente `vTaskDelay(...)` de `hello_task`.
  - *¿Qué le pasa al LED?* El sistema nunca se bloquea, por lo que `hello_task` (si tiene igual o mayor prioridad) satura el CPU creando un cuello de botella. El LED deja de parpadear.
  - *¿Por qué ayuda el bloqueo (delay)?* Al regresar el `vTaskDelay`, el LED vuelve a parpadear cada 300 ms y los mensajes seriales salen cada 1 s. El bloqueo permite liberar el CPU para que el *scheduler* ejecute otras tareas en espera.

### Lab 02: Queues (Producer / Consumer)
- **Make the producer faster (200ms → 20ms):** - *¿Cuándo aparece "Queue full"?* Cuando el productor genera datos más rápido de lo que el consumidor puede procesarlos. Al ser el consumidor más lento, los espacios en la cola se llenan rápidamente.
- **Increase the queue length (5 → 20):**
  - *¿Qué cambia?* El consumidor tiene un "colchón" o *buffer* más grande para almacenar datos. Sin embargo, una vez llenos esos 20 espacios, comenzará a perder datos si el consumidor sigue siendo lento.
- **Make the consumer “slow” (Delay 300ms):** - *¿Qué patrón ocurre (buffering/backlog)?* En los primeros segundos, el productor llena los 20 espacios de la cola. Como el consumidor tarda 300ms en procesar 1 dato, en ese mismo tiempo el productor intentó enviar 15 datos nuevos (300 / 20 = 15). La cola siempre estará llena (Backlog).

### Lab 03: Race Conditions & Mutex
- **Part A (Race Demo):** - *¿Por qué el contador es incorrecto?* La Tarea A copia el valor a su memoria local. El sistema pausa la Tarea A antes de guardar y le da turno a la Tarea B. La Tarea B lee el mismo valor (ej. 100), lo incrementa a 101 y lo guarda. Cuando la Tarea A regresa, sigue pensando que el valor era 100, lo incrementa a 101 y sobrescribe el valor de B. Se hicieron dos incrementos, pero el contador solo subió 1. Se "perdió" información.
- **Remove the mutex:** - *¿Ves comportamiento extraño?* Sí, el contador crece más lento de lo esperado o muestra valores inconsistentes.
- **Change priorities (TaskA=6, TaskB=4):** - *Expectativa:* La Tarea A interrumpirá a la Tarea B casi siempre que termine su tiempo de espera debido a su mayor prioridad.
- **What does a mutex guarantee?**
  - Un Mutex garantiza que *solo una tarea* pueda acceder a un recurso compartido en un momento dado.

---

## 6) Código Fuente

### Code Lab 01 (Tasks)
```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
#include "esp_log.h"

#define LED_GPIO GPIO_NUM_2   

static const char *TAG = "LAB1";

static void blink_task(void *pvParameters) {
    gpio_reset_pin(LED_GPIO);
    gpio_set_direction(LED_GPIO, GPIO_MODE_OUTPUT);

    while (1) {
        gpio_set_level(LED_GPIO, 1);
        vTaskDelay(pdMS_TO_TICKS(300));
        gpio_set_level(LED_GPIO, 0);
        vTaskDelay(pdMS_TO_TICKS(300));
    }
}

static void hello_task(void *pvParameters) {
    int n = 0;
    while (1) {
        ESP_LOGI(TAG, "hello_task says hi, n=%d", n++);
        vTaskDelay(pdMS_TO_TICKS(1000));
    }
}

void app_main(void) {
    ESP_LOGI(TAG, "Starting Lab 1 (two tasks)");
    // blink_task prioridad mas alta
    xTaskCreate(blink_task, "blink_task", 2048, NULL, 5, NULL);
    // hello_task prioridad baja (antes era 5)
    xTaskCreate(hello_task, "hello_task", 2048, NULL, 2, NULL);
}