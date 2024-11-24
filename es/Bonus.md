Los bonus se calculan en función de ciertas condiciones especificadas. Cada tipo de bonus te ayudará a contar los puntos extras de la cartas según el propio tipo.

Existe un campo en el bonus que es común para todos los tipos de bonus que es `affectCards`. Este puede tener diferentes casos:

- **allExceptMe**: Para calcular el bonus se usan todas las cartas de tu mano, excepto la propia carta que está calculando el valor del bonus. Este es el valor por defecto
- **all**:  Para calcular el bonus se usan todas las cartas, incluyendo la propia carta que calcula el bonus

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
  - **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. En el ejemplo, `"or"`.
  - **conditions**: más información [aquí](es/Condiciones.md)
  - **multiply**: Multiplicador para los puntos. En el ejemplo, `1`.

Cada entrada en la lista **countCards** representa un conjunto diferente de condiciones y un multiplicador correspondiente para los puntos.

##### 2. En Colección

Este tipo de bonus añade puntos extras a la carta en cuestión en el caso que tengas otras cartas en tu mano. Por ejemplo, en el ejemplo siguiente para "Fantasy Realms", ganarás 50 puntos en el caso que en tu mano tengas la carta con idenficador `incendio` o `humo`

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "inCollection": {
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
  - **strategy**: Estrategia para evaluar las condiciones. Puede ser: 
        - `"oneCardFulfillSomeCondition"` (por defecto) donde una carta debe cumplir alguna condición
        - `"oneCardFulfillAllTheConditions"` donde una carta debe cumplir todas las condiciones
        - `"eachConditionIsFulfilledByACard"` donde cada condición debe ser cumplida al menos por una carta diferente
        - `"eachConditionIsFulfilledByAllCards"` donde cada condición debe ser cumplida por todas las cartas
        - `"unlessOneCardFulfillAllTheConditions"` a menos que tengas una carta que cumpla todas las condiciones
  - **conditions**: más información [aquí](es/Condiciones.md)
  - **points**: Puntos adicionales otorgados si se cumplen las condiciones especificadas. En el ejemplo, `50`.

##### 3. Contar Otros Ítems

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

##### 4. Combinar diferentes bonus

Este tipo de bonus combina diferentes bonus para elegir uno de ellos. Por ejemplo, puedes tener
diferentes opciones y eligir cual de ellas es la mejor (te da mas puntos).

Dentro de estas opciones podrás encontrar 2 listas:

1. `countCardsOptions`: una lista de bonus "Contar Cartas" (descrita en sección "5.c.1")
2. `inCollectionOptions`: una lista de bonus "En Colección" (descrita en sección "5.c.2")

Para el siguiente ejemplo tenemos 2 opciones:

1. Ganas 15 puntos si tienes una carta de `type` igual a `leader` 
2. Ganas 40 puntos si además de tener una carta de `type` igual a `leader` tienes también la carta con identificador `espada_keth`

De los 2 casos, este bonus te añadirá el caso con mayor puntuación, ya que en el ejemplo el `type` es igual a `best`

**Ejemplo en JSON:**
```json
{
    "bonus": {
        "fromOptions": {
            "type": "best",
            "inCollectionOptions": [
                {
                    "conditions": [
                        {
                            "condition": "equal",
                            "field": "type",
                            "value": "leader"
                        }
                    ],
                    "points": 10
                },
                {
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
                            "value": "escudo_keth"
                        }
                    ],
                    "points": 40
                }
            ]
        }
    }
}
```

Los nuevos campos son:

- **type**: Define cómo se seleccionará la mejor opción de las condiciones dadas. Puede ser:
  - `"best"`: Selecciona la opción con la mejor puntuación.
  - `"worst"`: Selecciona la opción con la peor puntuación.

En el ejemplo, el tipo es `"best"`, lo que significa que se seleccionará la opción con la mayor puntuación de todas las que cumplan las condiciones.