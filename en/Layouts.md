The "layouts" section in the JSON file contains information on how to modify the appearance of the selected cards list and the dialog where a card is selected. Below is the structure and fields of this section.

**Example in JSON:**
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

#### Fields in the "Layouts" Section

##### a. Colors

**Example in JSON:**
```json
{
    "type": "border",
    "selectedCards": true,
    "dialogCards": true,
    "darkTheme": true,
    "lightTheme": true
}
```

- **type**: Defines the type of layout. It can be `"border"` or `"background"`. In the example, `"border"` indicates that it is a border.
- **selectedCards**: Indicates whether the configuration applies to the selected cards. In the example, `true`.
- **dialogCards**: Indicates whether the configuration applies to the card selection dialog. In the example, `true`.
- **darkTheme**: Indicates whether the configuration applies in dark mode. In the example, `true`.
- **lightTheme**: Indicates whether the configuration applies in light mode. In the example, `true`.
- **options**: List of layout options based on conditions.

##### b. Options

Each option within "options" has the following structure:

**Example in JSON:**
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

- **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. Default is `"and"`. In the example, `"and"`.
- **conditions**: more information [here](en/Conditions.md)
- **color**: Color in ARGB format. The first two hexadecimal digits represent the alpha value. Example: `"ff5f39a5"`.
- **borderWidth**: Border width in pixels. Applies only if the `type` is `"border"`. In the example, `10`.
- **borderSide**: Side of the border where the color is applied. Applies only if the `type` is `"border"`. Possible values are `"left"`, `"right"`, `"top"`, and `"bottom"`. In the example, `"left"`.

