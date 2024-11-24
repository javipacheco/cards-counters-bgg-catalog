El apartado "anular" contiene información sobre cómo anular las propiedades de otras o la propia carta. Una carta que tiene anulaciones aplicará estas condiciones a las cartas que has bajado, haciendo que no tengan tipo, ni puntos base, ni bonus, ni penalizaciones. A continuación se detalla la estructura y los campos de este apartado.

En este caso del juego "Fantasy Realms" si tienes esta carta en mano anula todas las cartas donde `type` es `army`, `leader` y `beast` (en este último caso menos la carta con identificador `basilisco`). Para todas esas cartas no se contarán los puntos base, bonux ni penalizaciones.

**Ejemplo en JSON:**
```json
{
    "blanks": [
        {
            "conditions": [
                {
                    "condition": "equal",
                    "field": "type",
                    "value": "army"
                }
            ]
        },
        {
            "conditions": [
                {
                    "condition": "equal",
                    "field": "type",
                    "value": "leader"
                }
            ]
        },
        {
            "operator": "and",
            "conditions": [
                {
                    "condition": "equal",
                    "field": "type",
                    "value": "beast"
                },
                {
                    "condition": "notEqual",
                    "field": "id",
                    "value": "basilisco"
                }
            ]
        }
    ]
}
```

Hay 3 tipos de estrategias para anular cartas:

##### 1. Anular otras cartas

Estrategia por defecto. Anula otras cartas que tienes en tu mano

**Ejemplo en JSON:**
```json
{
    "strategy": "otherCards",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "army"
        }
    ]
}
```

- **conditions**: Lista de condiciones que deben cumplirse para que una carta sea anulada.
  - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
  - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "type", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"type"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"army"`.

##### 2. Anula la propia carta a menos que tenga cartas

Anula la propia carta a menos que tenga cartas que cumplan la condición

**Ejemplo en JSON:**
```json
{
    "strategy": "myselfAtLeast",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "flame"
        }
    ]
}
```

##### 3. Anula la propia carta si tiene cartas

Anula la propia carta si tiene cartas que cumplen la condición

**Ejemplo en JSON:**
```json
{
    "strategy": "myselfWith",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "flood"
        }
    ]
}
```
