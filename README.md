# SumoBot UTEQ – Robot Sumo Autónomo y Manual con STM32

Proyecto Final de Sistemas Embebidos (Etapa 1)
Universidad Tecnológica de Querétaro (UTEQ) – Periodo Sep-Dic 2026

## Integrantes

- Luis Jair Campuzano Morales
- Samuel Daniel Monrony Hernadez​

## Descripción general

Robot sumo controlado por un microcontrolador **STM32F407 (STM32F407G-DISC1)** que puede operar en dos modos:

- **Modo autónomo:** detecta al oponente, lo empuja fuera del dojo circular de borde blanco y evita salirse él mismo.
- **Modo manual:** se controla desde el celular por Bluetooth (módulo HC-05 por UART).

La velocidad de los motores se regula con un **control PID** usando encoders como retroalimentación.

## Problemática y justificación

El robot sumo es un reto clásico de robótica móvil que integra en un solo sistema sensado, control en tiempo real y accionamiento de motores. Permite aplicar los temas de la materia: manejo de GPIO (entradas digitales), temporizadores con PWM, comunicación UART, control PID y diseño de máquinas de estados en un microcontrolador STM32.

## Arquitectura del sistema

**Entradas**
- Sensores IR de línea (2 a 4): detectan el borde blanco del dojo (entradas digitales).
- Sensores de distancia para detectar al oponente (2 a 3): frontal y laterales.
- Botón de inicio (entrada digital) con retardo de arranque de 5 s.
- Módulo Bluetooth HC-05 (UART): recibe los comandos del modo manual.
- Encoders de los motores: miden la velocidad para el control PID.

**Procesamiento**
- STM32F407 con máquina de estados (espera, búsqueda, ataque, evasión de borde), modo manual y control PID.

**Salidas**
- 2 motores DC con reductor controlados por **PWM** (timers del STM32) mediante un driver de puente H.
- LEDs de estado (opcional).

**Alimentación**
- Batería LiPo 2S (7.4 V) para motores, con reguladores para la lógica (5 V / 3.3 V).

## Lógica de control

**Modo autónomo**
1. **Espera:** el robot permanece detenido hasta presionar el botón y transcurre el retardo de 5 s.
2. **Búsqueda:** gira en su lugar hasta detectar al oponente.
3. **Ataque:** avanza con PWM alto hacia el oponente.
4. **Evasión de borde:** si un sensor de línea detecta blanco, retrocede y gira para volver al centro. Tiene prioridad sobre los demás estados.

**Modo manual**
- Se activa con un comando Bluetooth. Desde el celular se envían comandos para avanzar, retroceder, girar y detener.

**Control PID**
- Los encoders miden la velocidad de cada motor y el STM32 ajusta el PWM para mantenerla en el valor deseado.

## Plan por etapas

| Etapa | Objetivo |
|---|---|
| 1 (actual) | Presentación de la propuesta, viabilidad y arquitectura |
| 2 | Demostración del 75% de los módulos funcionando de forma aislada y 50% de avance en el prototipo físico |
| 3 | Entrega final: funcionamiento al 100%, prototipo completo y revisión del código fuente |

## Estructura del repositorio

```
/
├── README.md
└── presentacion/
    └── Presentacion_Etapa1_SumoBot.pptx
```

## Herramientas

- Microcontrolador: STM32F407G-DISC1
- Entorno de desarrollo: [STM32CubeIDE]
- Lenguaje: C
