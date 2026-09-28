# Caja de Recaudo con Tope de Seguridad

**Integrantes:**

- Adrian David Santos Buitrago
- Freddy Stiven Rueda Hernandez
- Breyner Cermeño Castellanos

**Asignatura:** Programación 1
**Docente:** Ing. Carlos Carrascal

---

## Descripción

Módulo de control de caja para **PagaYa S.A.S.**, empresa que recauda pagos de facturas de servicios para terceros. Por política de seguridad, cada cajero tiene un límite de efectivo que puede acumular durante su turno. Al alcanzarlo o superarlo, la caja se suspende para que el efectivo sea retirado.

El programa está desarrollado en **Python**, usa **archivos CSV** como base de datos e implementa un **CRUD** completo sobre turnos y transacciones. Está pensado para ejecutarse en **Google Colab**, donde los CSV se pueden abrir y modificar directamente.

## Requerimientos funcionales

| Código | Requerimiento |
|--------|---------------|
| RF1 | **Inicio de turno.** Solicita el nombre o código del cajero y el tope máximo de recaudo (debe ser mayor que cero). |
| RF2 | **Atención de clientes.** Mientras la caja esté activa, atiende en orden de llegada, acumula el total y cuenta cada transacción. |
| RF3 | **Validación de montos.** Rechaza montos menores o iguales a cero y valores no numéricos. Un monto rechazado no se cuenta ni se acumula. |
| RF4 | **Suspensión por seguridad.** Al llegar o superar el tope se muestra el mensaje de suspensión y se dejan de aceptar transacciones. |
| RF5 | **Cierre por fin de cola.** Si el cajero digita `0` o `FIN` antes de alcanzar el tope, el turno termina normalmente. |
| RF6 | **Reporte final.** Cajero, tope, número de transacciones, recaudo total, promedio por transacción y motivo de cierre. |

## Reglas de negocio

- La transacción que cruza el tope **sí se registra** y luego se suspende la caja, por lo que el recaudo total puede ser mayor que el tope.
- Condición de suspensión: `recaudoTotal >= tope`.
- Moneda: pesos colombianos (COP), valores enteros.
- Formato del reporte: miles con separador de punto y promedio con dos decimales.
- Si no hubo transacciones, el promedio es 0 (sin división por cero).

## Funcionalidades (CRUD)

| Operación | Opción del menú | Descripción |
|-----------|-----------------|-------------|
| **Create** | 1. Iniciar turno | Registra un turno con sus transacciones y genera el reporte. |
| **Read** | 2, 3, 4 | Lista turnos, muestra el reporte de un turno y consulta sus transacciones. |
| **Update** | 5, 6 | Modifica cajero o tope de un turno, o el monto de una transacción (recalcula el turno). |
| **Delete** | 7, 8 | Elimina un turno con sus transacciones, o una transacción individual (recalcula el turno). |

## Base de datos (CSV)

El programa crea automáticamente estos archivos si no existen:

**`turnos.csv`**

| Campo | Descripción |
|-------|-------------|
| `idTurno` | Identificador único del turno |
| `cajero` | Nombre o código del cajero |
| `tope` | Tope máximo de recaudo asignado |
| `numTransacciones` | Cantidad de transacciones válidas |
| `recaudoTotal` | Total recaudado |
| `promedio` | Monto promedio por transacción |
| `motivoCierre` | `Tope alcanzado` o `Fin de cola` |

**`transacciones.csv`**

| Campo | Descripción |
|-------|-------------|
| `idTransaccion` | Identificador único de la transacción |
| `idTurno` | Turno al que pertenece |
| `monto` | Monto recibido |

## Ejecución en Google Colab

1. Abre [Google Colab](https://colab.research.google.com/) y crea un nuevo notebook.
2. Copia el contenido de `cajaRecaudo.py` en una celda (o sube el archivo desde el panel de archivos de la izquierda).
3. Ejecuta la celda e interactúa con el menú desde la consola.
4. Los archivos `turnos.csv` y `transacciones.csv` aparecerán en el panel de archivos, donde puedes abrirlos y editarlos.

> **Nota:** los archivos de Colab son temporales. Descárgalos antes de cerrar la sesión si quieres conservar los datos.

## Ejecución local

```bash
python cajaRecaudo.py
```

## Ejemplo de ejecución

**Entrada:** cajero `C-102`, tope `$1.000.000`

| Cliente | Monto | Acumulado |
|---------|-------|-----------|
| 1 | 350.000 | 350.000 |
| 2 | -20.000 | rechazado |
| 3 | 400.000 | 750.000 |
| 4 | 300.000 | 1.050.000 (se supera el tope) |

**Salida esperada:**

```
CAJA SUSPENDIDA: se alcanzó el tope de recaudo. Diríjase a tesorería.

===== REPORTE DE TURNO =====
Cajero:                C-102
Tope asignado:         $1.000.000
Transacciones:         3
Recaudo total:         $1.050.000
Promedio/transacción:  $350.000,00
Motivo de cierre:      Tope alcanzado
============================
```

## Casos de prueba sugeridos

- Tope alcanzado exactamente (recaudo = tope).
- Tope superado por la última transacción.
- Fin de cola sin alcanzar el tope.
- Turno sin ninguna transacción (verificar que no haya división por cero).
- Montos inválidos intercalados (cero, negativos, texto).
- Tope inválido al inicio (cero o negativo).

## Estructura del repositorio

```
.
├── cajaRecaudo.py        # Código fuente principal
├── turnos.csv            # Base de datos de turnos (se genera al ejecutar)
├── transacciones.csv     # Base de datos de transacciones (se genera al ejecutar)
└── README.md
```

## Convenciones de código

- Variables y funciones en **camelCase**.
- Comentarios con `#` que explican cada bloque.
