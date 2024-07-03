
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

Información adicional específica del juego. Puedes añadir la información que veas necesaria para luego usar en las condiciones de los "Bonus" por ejemplo

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

# 5. Bonux

Los bonus se calculan en función de ciertas condiciones especificadas.

### a. Contar Cartas

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "countCards": [
            {
                "conditions": [
                    {
                        "condition": "equal",
                        "field": "cardType",
                        "value": "purple_prosperity"
                    }
                ],
                "multiply": 1
            }
        ]
    }
}
```

- **countCards**: Cuenta las cartas según las condiciones especificadas y añade puntos extra según el multiplicador.
  - **conditions**: Lista de condiciones que deben cumplirse para que las cartas sean contadas.
    - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
    - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "points", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"cardType"`.
    - **value**: Valor que debe tener el campo. Ejemplo: `"purple_prosperity"`.
  - **multiply**: Multiplicador para los puntos. En el ejemplo, `1`.

Cada entrada en la lista **countCards** representa un conjunto diferente de condiciones y un multiplicador correspondiente para los puntos.

#### b. En Colección

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "inCollection": {
            "conditions": [
                {
                    "condition": "equal",
                    "field": "id",
                    "value": "esposo"
                }
            ],
            "points": 3
        }
    }
}
```

- **inCollection**: Comprueba si se cumplen ciertas condiciones para las cartas en tu colección.
  - **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. Por defecto es `"and"`. En el ejemplo, `"and"`.
  - **conditions**: Lista de condiciones que deben cumplirse.
    - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
    - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "points", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"id"`.
    - **value**: Valor que debe tener el campo. Ejemplo: `"esposo"`.
  - **points**: Puntos adicionales otorgados si se cumplen las condiciones especificadas. En el ejemplo, `3`.

#### c. Contar Otros Ítems

**Ejemplo en JSON:**
```json
{
    "bonus": {
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


### 6. Diseños (Layouts)

El apartado "layouts" en el archivo JSON contiene información sobre cómo modificar el aspecto del listado de cartas seleccionadas y el diálogo donde se selecciona una carta. A continuación se detalla la estructura y los campos de este apartado.

**Ejemplo en JSON:**
```json
{
    "layouts": {
        "colors": [
            {
                "type": "border",
                "selectedCards": true,
                "dialogCards": true,
                "darkTheme": true,
                "lightTheme": true,
                "options": [
                    {
                        "conditions": [
                            {
                                "condition": "equal",
                                "field": "cardType",
                                "value": "purple_prosperity"
                            }
                        ],
                        "color": "ff5f39a5",
                        "borderWidth": 10,
                        "borderSide": "left"
                    }
                ]
            }
        ]
    }
}
```

#### Campos del Apartado "Layouts"

##### a. Colores

**Ejemplo en JSON:**
```json
{
    "type": "border",
    "selectedCards": true,
    "dialogCards": true,
    "darkTheme": true,
    "lightTheme": true
}
```

- **type**: Define el tipo de diseño. Puede ser `"border"` o `"background"`. En el ejemplo, `"border"` indica que se trata de un borde.
- **selectedCards**: Indica si la configuración se aplica a las cartas seleccionadas. En el ejemplo, `true`.
- **dialogCards**: Indica si la configuración se aplica al diálogo de selección de cartas. En el ejemplo, `true`.
- **darkTheme**: Indica si la configuración se aplica en el tema oscuro. En el ejemplo, `true`.
- **lightTheme**: Indica si la configuración se aplica en el tema claro. En el ejemplo, `true`.
- **options**: Lista de opciones de diseño basadas en condiciones.

##### b. Opciones

Cada opción dentro de "options" tiene la siguiente estructura:

**Ejemplo en JSON:**
```json
{
    "operator": "and",
    "conditions": [
        {
            "condition": "equal",
            "field": "cardType",
            "value": "purple_prosperity"
        }
    ],
    "color": "ff5f39a5",
    "borderWidth": 10,
    "borderSide": "left"
}
```

- **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. Por defecto es `"and"`. En el ejemplo, `"and"`.
- **conditions**: Lista de condiciones que deben cumplirse para aplicar esta opción.
  - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
  - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "points", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"cardType"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"purple_prosperity"`.
- **color**: Color en formato ARGB. Los primeros dos dígitos hexadecimales representan el valor alfa. Ejemplo: `"ff5f39a5"`.
- **borderWidth**: Ancho del borde en píxeles. Solo se aplica si el `type` es `"border"`. En el ejemplo, `10`.
- **borderSide**: Lado del borde donde se aplica el color. Solo se aplica si el `type` es `"border"`. Los valores posibles son `"left"`, `"right"`, `"top"` y `"bottom"`. En el ejemplo, `"left"`.
