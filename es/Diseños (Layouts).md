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
