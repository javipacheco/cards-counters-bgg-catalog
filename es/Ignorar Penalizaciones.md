El apartado permite especificar cartas cuyas penalizaciones serán ignoradas. Todo lo ignorado ocurre antes de que la penalización sea aplicada.

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
  - `"valueOfCondition"`: ignora el valor de una condición en una penalización. Ejemplo más abajo
- **numberOfCards**: Número de cartas que el usuario debe seleccionar en caso de que el tipo sea `"some"`. En el ejemplo, `1`.
- **valueToIgnore**: Palabra a ignorar en el caso de que el tipo sea `"valueOfCondition"`
- **operator**: Operador que determina cómo se evalúan las condiciones. Puede ser `"and"` o `"or"`. En el ejemplo, `"or"`.
- **conditions**: Lista de condiciones que deben cumplirse para que una carta sea ignorada.
  - **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
  - **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como "type" o "id", o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"type"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"flood"`.
  - **value**: Valor que debe tener el campo. Ejemplo: `"flame"`.

**Ejemplo en JSON para `valueOfCondition`:**

Este ejemplo ignora la palabra `army` de todas las penalizaciones excepto `dirigible_guerra`

```json
{
    "id": "exploradores",
    "points": 5,
    "clears": [
        {
            "type": "valueOfCondition",
            "valueToIgnore": "army",
            "conditions": [
                {
                    "condition": "notEqual",
                    "field": "id",
                    "value": "dirigible_guerra"
                }
            ]
        }
    ]
}
```
