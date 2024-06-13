
# Manual para Generar un Sistema de Puntuación para Juegos de Mesa

Este manual proporciona una guía detallada sobre cómo generar un sistema de puntuación para juegos de mesa utilizando un archivo JSON. A continuación se presenta la estructura del archivo JSON con ejemplos específicos para el juego "Everdell".

## Estructura del JSON

### 1. Información del Juego

**Ejemplo en JSON:**
```json
{
    "game": "Everdell",
    "maxCards": null
}
```

- **game**: Nombre del juego. En el ejemplo, `"Everdell"`.
- **maxCards**: Número máximo de cartas permitidas. Puede ser `null` si no hay un límite específico. En el ejemplo, `null`.

### 2. Traducciones

Contiene las traducciones de mensajes y nombres de cartas en diferentes idiomas.

#### a. Mensajes

**Ejemplo en JSON:**
```json
{
    "translations": {
        "others": {
            "arquitecto_msg": {
                "es": "por cada resina y guijarro, máximo 6",
                "en": "for each resin and pebble, maximum 6"
            },
            "rey_msg_1": {
                "es": "por cada Evento Básico",
                "en": "for each Basic Event"
            },
            "rey_msg_2": {
                "es": "por cada Evento Especial",
                "en": "for each Special Event"
            }
        }
    }
}
```

- **translations.others**: Son textos generales, donde se usará la llave, por ejemplo `arquitecto_msg` y será traducido al idioma correspodiente.

#### b. Cartas

**Ejemplo en JSON:**
```json
{
    "translations": {
        "cards": {
            "arbol_eterno": {
                "es": "Árbol Eterno",
                "en": "Ever Tree"
            },
            "esposa": {
                "es": "Esposa",
                "en": "Wife"
            }
        }
    }
}
```

- **translations.cards**: Traducciones de los nombres de las cartas.
  - **[id_de_carta]**: Traducción del nombre de la carta.
    - **es**: Nombre en español. Ejemplo: `"Árbol Eterno"`
    - **en**: Nombre en inglés. Ejemplo: `"Ever Tree"`

### 3. Cajas de Cartas (Expansiones)

Contiene la información sobre las diferentes expansiones de cartas disponibles en el juego.

**Ejemplo en JSON:**
```json
{
    "boxes": [
        {
            "name": "base",
            "cards": [
                {
                    "id": "arbol_eterno",
                    "points": 5,
                    "type": "construction",
                    "unique": true,
                    "metadata": {
                        "cardType": "purple_prosperity",
                        "twigs": 3,
                        "resin": 3,
                        "pebbles": 3
                    }
                }
            ]
        }
    ]
}
```

- **name**: Nombre de la expansión. En el ejemplo, `"base"`.
- **cards**: Lista de cartas incluidas en esta expansión.

### 4. Cartas

Cada carta tiene la siguiente estructura:

#### a. Identificación

**Ejemplo en JSON:**
```json
{
    "id": "arbol_eterno"
}
```

- **id**: Identificador único de la carta. En el ejemplo, `"arbol_eterno"`.

#### b. Puntuación

**Ejemplo en JSON:**
```json
{
    "points": 5
}
```

- **points**: Puntos base de la carta. En el ejemplo, `5`.

#### c. Tipo de Carta

**Ejemplo en JSON:**
```json
{
    "type": "construction"
}
```

- **type**: Tipo de carta (e.g., "construction", "critter"). En el ejemplo, `"construction"`.

#### d. Unicidad

**Ejemplo en JSON:**
```json
{
    "unique": true
}
```

- **unique**: Indica si la carta es única. En el ejemplo, `true`.

#### e. Metadatos

Información adicional específica del juego. Puedes añadir la información que veas necesaria para luego usar en las condiciones de los "Puntos Extras" por ejemplo

**Ejemplo en JSON:**
```json
{
    "metadata": {
        "cardType": "purple_prosperity",
        "twigs": 3,
        "resin": 3,
        "pebbles": 3
    }
}
```

### 5. Puntos Extras

Los puntos extras se calculan en función de ciertas condiciones especificadas.

#### a. Contar Cartas

**Ejemplo en JSON:**
```json
{
    "extraPoints": {
        "countCards": {
            "conditions": [
                {
                    "condition": "equal",
                    "field": "cardType",
                    "value": "purple_prosperity"
                }
            ],
            "multiply": 1
        }
    }
}
```

- **countCards**: Cuenta las cartas según las condiciones y añade esos puntos extras.
  - **conditions**: Lista de condiciones que deben cumplirse.
    - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`
    - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "points", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"cardType"`
    - **value**: Valor que debe tener el campo. Ejemplo: `"purple_prosperity"`
  - **multiply**: Factor de multiplicación para los puntos. Ejemplo: `1`

#### b. En Colección

**Ejemplo en JSON:**
```json
{
    "extraPoints": {
        "inCollection": {
            "id": "esposo",
            "points": 3
        }
    }
}
```

- **inCollection**: Comprueba si el "id" está en tu lista de cartas.
  - **id**: Identificador de la otra carta. En el ejemplo, `"esposo"`
  - **points**: Puntos adicionales. En el ejemplo, `3`

#### c. Contar Otros Ítems

**Ejemplo en JSON:**
```json
{
    "extraPoints": {
        "countOtherItems": [
            {
                "translation": "arquitecto_msg",
                "max": 6,
                "multiply": 1
            }
        ]
    }
}
```

- **countOtherItems**: Usado normalmente para contar puntos al final de la partida según el número de recursos que tienes, objetivos alcanzados, etc. Este dato tiene que añadirlo el jugador a mano y no puede ser calculado automáticamente.
  - **translation**: Mensaje de traducción obteneido desde `translations.others`. En el ejemplo, `"arquitecto_msg"`
  - **max**: Máximo de ítems a contar. En el ejemplo, `6`
  - **multiply**: Factor de multiplicación para los puntos. En el ejemplo, `1`
