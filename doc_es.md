
# Manual para Generar un Sistema de Puntuación para Juegos de Mesa

Este manual proporciona una guía detallada sobre cómo generar un sistema de puntuación para juegos de mesa utilizando un archivo JSON. A continuación se presenta la estructura del archivo JSON con ejemplos específicos para el juego "Everdell" y "Fantasy Realms".

## Estructura del JSON

### 1. Información del Juego

Lo primero es ponerle nombre al juego.

**Ejemplo en JSON:**
```json
{
    "game": "Everdell",
    "maxCards": null
}
```

- **game**: Nombre del juego. En el ejemplo, `"Everdell"`.
- **maxCards**: Número máximo de cartas permitidas. Puede ser `null` si no hay un límite específico. En el ejemplo, `null`.

### 2. Cajas de Cartas (Expansiones)

Contiene la información sobre las diferentes expansiones de cartas disponibles en el juego. El apartado `"boxes"` contiene las cajas del juego. En muchos casos habrá sólo una, la caja "base", pero en otras se podría añadir las cartas de las expansiones. Cada caja contendrá el nombre de la caja y las cartas que trae. 

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
- **cards**: Lista de cartas incluidas en esta caja.

### 3. Cartas

Esta es la parte más importante del archivo, ya que contiene la información de cada una de las cartas. Cada carta tendrá unos campos comunes como un identificador o los puntos de esa carta, pero también tendrá una serie de metadatos que es información exclusiva para el juego que se está construyendo, por ejemplo "Everdell" tiene un el número de resinas que vale la carta y esta información es exclisiva para "Everdell". Además debemos añadir inforamción de los bonus que da la carta o si anula o ignora otras cartas de tu mano. Todo se explica a continuación.

Cada carta tiene la siguiente estructura:

#### a. Campos comunes

**Ejemplo en JSON:**
```json
{
    "id": "arbol_eterno",
    "points": 5,
    "type": "construction",
    "unique": true
}
```

- **id**: Identificador único de la carta. En el ejemplo, `"arbol_eterno"`.
- **points**: Puntos base de la carta. En el ejemplo, `5`.
- **type**: Tipo de carta (e.g., "construction", "critter"). En el ejemplo, `"construction"`.
- **unique**: Indica si la carta es única. En el ejemplo, `true`.

#### b. Metadatos

Información adicional específica del juego. Puedes añadir la información que veas necesaria para luego usar en las condiciones de los "Bonus" por ejemplo

Esto es un ejemplo de unos metadatos para "Everdell", pero aquí puedes añadir cualquier información relacionada con el juego.

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

Por ejemplo en este caso, como veremos en las condiciones, podremos preguntar las cartas donde `cardType` sea igual a `purple_prosperity`. Te puede imaginar esto para cualquier campo que tengas en tu juego

#### c. Bonus

Los bonus se calculan en función de ciertas condiciones especificadas. Cada tipo de bonus te ayudará a contar los puntos extras de la cartas según el propio tipo.

Existen 4 tipos de bonus:

##### 1. Contar Cartas

Este tipo de bonus añade puntos a la carta contando el número de otras cartas que tiene en tu mano. Por ejemplo en este caso, por cada carta que tengas que tu mano que cumpla la condición donde `cardType` sea igual a `purple_prosperity` te añadirá 1 punto extra.

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

##### 2. En Colección

Este tipo de bonus añade puntos extras a la carta en cuestión en el caso que tengas otras cartas en tu mano. Por ejemplo, en el ejemplo siguiente para "Fantasy Realms", ganarás 50 puntos en el caso que en tu mano tengas la carta con idenficador `incendio` o `humo`

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "inCollection": {
            "operator": "or",
            "strategy": "eachConditionIsFulfilledByACard",
            "conditions": [
                {
                    "condition": "equal",
                    "field": "id",
                    "value": "incendio"
                },
                {
                    "condition": "equal",
                    "field": "id",
                    "value": "humo"
                }
            ],
            "points": 50
        }
    }
}
```

- **inCollection**: Comprueba si se cumplen ciertas condiciones para las cartas en tu colección.
  - **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. Por defecto es `"and"`. En el ejemplo, `"or"`.
  - **strategy**: Estrategia para evaluar las condiciones. Puede ser `"eachConditionIsFulfilledByACard"` donde cada condición debe ser cumplida al menos por una carta, o `"oneCardFulfillAllTheConditions"` donde una carta debe cumplir todas las condiciones. Por defecto es `"oneCardFulfillAllTheConditions"`. En el ejemplo, `"eachConditionIsFulfilledByACard"`.
  - **conditions**: Lista de condiciones que deben cumplirse.
    - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
    - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "points", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"id"`.
    - **value**: Valor que debe tener el campo. Ejemplo: `"incendio"`.
    - **value**: Valor que debe tener el campo. Ejemplo: `"humo"`.
  - **points**: Puntos adicionales otorgados si se cumplen las condiciones especificadas. En el ejemplo, `50`.

##### 3. En Colección con Opciones

Este tipo de bonus es parecido al bonus "En Colección" pero añadirás diferentes opciones y sólo te dará la puntuación. En este caso de las 2 opciones:

1. Ganas 15 puntos si tienes una carta de `type` igual a `leader` 
2. Ganas 40 puntos si además de tener una carta de `type` igual a `leader` tienes también la carta con identificador `espada_keth`

De los 2 casos, este bonus te añadirá el caso con mayor puntuación, ya que en el ejemplo el `type` es igual a `best`

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "inCollectionWithOptions": {
            "type": "best",
            "options": [
                {
                    "conditions": [
                        {
                            "condition": "equal",
                            "field": "type",
                            "value": "leader"
                        }
                    ],
                    "points": 15
                },
                {
                    "operator": "or",
                    "strategy": "eachConditionIsFulfilledByACard",
                    "conditions": [
                        {
                            "condition": "equal",
                            "field": "type",
                            "value": "leader"
                        },
                        {
                            "condition": "equal",
                            "field": "id",
                            "value": "espada_keth"
                        }
                    ],
                    "points": 40
                }
            ]
        }
    }
}
```

Cada opción dentro de **inCollectionWithOptions** es igual a las condiciones descritas en la sección 5.b "En Colección".

- **type**: Define cómo se seleccionará la mejor opción de las condiciones dadas. Puede ser:
  - `"best"`: Selecciona la opción con la mejor puntuación.
  - `"worst"`: Selecciona la opción con la peor puntuación.
  - `"first"`: Selecciona la primera opción en orden que cumpla la condición. 

En el ejemplo, el tipo es `"best"`, lo que significa que se seleccionará la opción con la mayor puntuación de todas las que cumplan las condiciones.

##### 4. Contar Otros Ítems

Usado normalmente para contar puntos al final de la partida según el número de recursos que tienes, objetivos alcanzados, etc. Este dato tiene que añadirlo el jugador a mano y no puede ser calculado automáticamente. Por ejemplo en este caso, en "Everdell" el usuario tiene que contar los recursos que tiene al final de la partida y añadirlos, esos serán los puntos a añadir como bonus, con un máximo de 6.

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

- **translation**: Mensaje de traducción obteneido desde `translations.others`. En el ejemplo, `"arquitecto_msg"`
- **max**: Máximo de ítems a contar. En el ejemplo, `6`
- **multiply**: Factor de multiplicación para los puntos. En el ejemplo, `1`


#### d. Anular

El apartado "anular" contiene información sobre cómo anular las propiedades de otras cartas. Una carta que tiene anulaciones aplicará estas condiciones a las cartas que has bajado, haciendo que no tengan tipo, ni puntos base, ni bonus, ni penalizaciones. A continuación se detalla la estructura y los campos de este apartado.

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

A continuación se describen los campos del apartado "Anular"

##### 1. Lista de condiciones para anular otras cartas

Cada entrada dentro de "anular" tiene la siguiente estructura:

**Ejemplo en JSON:**
```json
{
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

##### 2. Condiciones Combinadas

Las condiciones pueden combinarse utilizando un operador lógico.

**Ejemplo en JSON:**
```json
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
```

- **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. Por defecto es `"and"`. En el ejemplo, `"and"`.
- **conditions**: Lista de condiciones que deben cumplirse para que una carta sea anulada.
  - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
  - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "id" o "type", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"type"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"beast"`.
  - **condition**: Tipo de condición. Ejemplo: `"notEqual"`.
  - **field**: Campo sobre el cual se aplica la condición. Ejemplo: `"id"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"basilisco"`.


#### e. Ignorar Penalizaciones

El apartado permite especificar cartas cuyas penalizaciones serán ignoradas. Todas los ignorado ocurre antes de que la penalización sea aplicada.

En el siguiente caso del juego "Fantasy Realms" el usuario puede seleccionar una carta para ignorar sus penalizaciones, pero no puede elegir cualquier carta, la carta que puede elegir está dentro de las condiciones, en este caso que el `type` sea `flood` o `flame`. En la aplicación se habilitará un botón para que el usuario puede seleccionar la carta.

**Ejemplo en JSON:**
```json
{
    "clears": [
        {
            "type": "some",
            "numberOfCards": 1,
            "operator": "or",
            "conditions": [
                {
                    "condition": "equal",
                    "field": "type",
                    "value": "flood"
                },
                {
                    "condition": "equal",
                    "field": "type",
                    "value": "flame"
                }
            ]
        }
    ]
}
```

A continuación se describen los campos del apartado "Ignorar"

- **type**: Define cómo se aplicarán las condiciones. Puede ser:
  - `"all"`: Ignora todos los valores de las cartas que cumplan las condiciones.
  - `"some"`: El usuario debe seleccionar 1 o más cartas dentro de las que cumplan las condiciones.
- **numberOfCards**: Número de cartas que el usuario debe seleccionar en caso de que el tipo sea `"some"`. En el ejemplo, `1`.
- **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. En el ejemplo, `"or"`.
- **conditions**: Lista de condiciones que deben cumplirse para que una carta sea ignorada.
  - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
  - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "type" o "id", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"type"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"flood"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"flame"`.

### 4. Traducciones

Contiene las traducciones de mensajes y nombres de cartas en diferentes idiomas. Los idiomas soportados por el momento son:

- Ingles (en)
- Español (es)
- Alemán (de)
- Portugués (pt)
- Italiano (it)
- Polaco (pl)
- Ruso (ru)

#### a. Mensajes

Estos mensaje se podrán usar en algunos bonus o campos de las cartas.

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

Estas son todas las traducciones de las cartas por tu identificador.

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

### 5. Diseños (Layouts)

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
