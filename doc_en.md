
# Manual for Generating a Scoring System for Board Games

This manual provides a detailed guide on how to generate a scoring system for board games using a JSON file. Below is the structure of the JSON file with specific examples for the game "Everdell".

## JSON Structure

### 1. Game Information

**Example in JSON:**
```json
{
    "game": "Everdell",
    "maxCards": null
}
```

- **game**: Name of the game. In the example, `"Everdell"`.
- **maxCards**: Maximum number of cards allowed. It can be `null` if there is no specific limit. In the example, `null`.

### 2. Translations

Contains translations of messages and card names in different languages.

#### a. Messages

**Example in JSON:**
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

- **translations.others**: These are general texts, where the key will be used, e.g. arquitecto_msg and will be translated into the corresponding language.

#### b. Cards

**Example in JSON:**
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

- **translations.cards**: Translations of the card names.
  - **[card_id]**: Translation of the card name.
    - **es**: Name in Spanish. Example: `"Árbol Eterno"`
    - **en**: Name in English. Example: `"Ever Tree"`

### 3. Card Boxes (Expansions)

Contains information about the different card expansions available in the game.

**Example in JSON:**
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

- **name**: Name of the expansion. In the example, `"base"`.
- **cards**: List of cards included in this expansion.

### 4. Cards

Each card has the following structure:

#### a. Identification

**Example in JSON:**
```json
{
    "id": "arbol_eterno"
}
```

- **id**: Unique identifier of the card. In the example, `"arbol_eterno"`.

#### b. Scoring

**Example in JSON:**
```json
{
    "points": 5
}
```

- **points**: Base points of the card. In the example, `5`.

#### c. Card Type

**Example in JSON:**
```json
{
    "type": "construction"
}
```

- **type**: Type of card (e.g., "construction", "critter"). In the example, `"construction"`.

#### d. Uniqueness

**Example in JSON:**
```json
{
    "unique": true
}
```

- **unique**: Indicates if the card is unique. In the example, `true`.

#### e. Metadata

Additional game-specific information. You can add the information you see necessary to be used in the conditions of the "Bonus", for example

**Example in JSON:**
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

### 5. Bonus

Bonus are calculated based on specific conditions.

#### a. Count Cards

**Example in JSON:**
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

- **countCards**: Counts cards based on specified conditions and adds extra points according to the multiplier.
  - **conditions**: List of conditions that must be met for the cards to be counted.
    - **condition**: Type of condition. It can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
    - **field**: Field to which the condition applies. It can be any common card field like "id" or "points", or any game-specific fields added in the "metadata" section. Example: `"cardType"`.
    - **value**: Value the field must have. Example: `"purple_prosperity"`.
  - **multiply**: Multiplier for the points. In the example, `1`.

Each entry in the **countCards** list represents a different set of conditions and a corresponding multiplier for the points.

#### b. In Collection

**Example in JSON:**
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

- **inCollection**: Checks if certain conditions are met for the cards in your collection.
  - **operator**: Operator that determines how conditions are evaluated. It can be `"and"` or `"or"`. The default is `"and"`. In the example, `"or"`.
  - **strategy**: Strategy for evaluating the conditions. It can be `"eachConditionIsFulfilledByACard"` where each condition must be met by at least one card, or `"oneCardFulfillAllTheConditions"` where one card must meet all conditions. The default is `"oneCardFulfillAllTheConditions"`. In the example, `"eachConditionIsFulfilledByACard"`.
  - **conditions**: List of conditions that must be met.
    - **condition**: Type of condition. It can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
    - **field**: Field to which the condition applies. It can be any common card field like "id" or "points", or any game-specific fields added in the "metadata" section. Example: `"id"`.
    - **value**: Value the field must have. Example: `"incendio"`.
    - **value**: Value the field must have. Example: `"humo"`.
  - **points**: Additional points awarded if the specified conditions are met. In the example, `50`.
 
#### c. In Collection with Options

**Example in JSON:**
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

Each option within **inCollectionWithOptions** is teh same to the conditions described in section 5.b "In Collection".

- **type**: Defines how the best option from the given conditions will be selected. It can be:
  - `"best"`: Selects the option with the best score.
  - `"worst"`: Selects the option with the worst score.
  - `"first"`: Selects the first option in order that meets the condition. 

In the example, the type is `"best"`, which means that the option with the highest score from all that meet the conditions will be selected.

#### d. Count Other Items

**Example in JSON:**
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

- **countOtherItems**: Usually used to count points at the end of the game based on the number of resources you have, goals achieved, etc. This data must be added manually by the player and cannot be calculated automatically.
  - **translation**: Translation message obtained from `translations.others`. In the example, `"arquitecto_msg"`
  - **max**: Maximum number of items to count. In the example, `6`
  - **multiply**: Multiplication factor for the points. In the example, `1`

### 6. Blanks

The "blanks" section contains information on how to nullify the properties of other cards. A card with blanks will apply these conditions to the cards you have played, causing them to have no base points, bonuses, or penalties. Below is the structure and fields of this section.

**Example in JSON:**
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
                    "value": "basilisk"
                }
            ]
        }
    ]
}
```

#### a. Blanks

Each entry in "blanks" has the following structure:

**Example in JSON:**
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

- **conditions**: List of conditions that must be met for a card to be blanked.
  - **condition**: Type of condition. It can be "equal", "notEqual", "greater", "less", "greaterOrEqual", "lessOrEqual". Example: "equal".
  - **field**: Field to which the condition applies. It can be any common card field like "id" or "type", or any game-specific fields added in the "metadata" section. Example: "type".
  - **value**: Value the field must have. Example: "army".

#### b. Combined Conditions

Conditions can be combined using a logical operator.

**Example in JSON:**
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
            "value": "basilisk"
        }
    ]
}
```

- **operator**: Operator that determines how conditions are evaluated. It can be "and" or "or". The default is "and". In the example, "and".
- **conditions**: List of conditions that must be met for a card to be blanked.
  - **condition**: Type of condition. It can be "equal", "notEqual", "greater", "less", "greaterOrEqual", "lessOrEqual". Example: "equal".
  - **field**: Field to which the condition applies. It can be any common card field like "id" or "type", or any game-specific fields added in the "metadata" section. Example: "type".
  - **value**: Value the field must have. Example: "beast".
  - **condition**: Type of condition. Example: "notEqual".
  - **field**: Field to which the condition applies. Example: "id".
  - **value**: Value the field must have. Example: "basilisk".


### 7. Clears

The "clears" section allows you to specify cards whose properties will be ignored. These properties are base points, penalties, and bonuses.

**Example in JSON:**
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

#### Fields in the "Clears" Section

Each entry in **clears** has the following structure:

**Example in JSON:**
```json
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
```

- **type**: Defines how the conditions will be applied. It can be:
  - `"all"`: Ignores all values of the cards that meet the conditions.
  - `"some"`: The user must select one or more cards from those that meet the conditions.
- **numberOfCards**: Number of cards that the user must select if the type is `"some"`. In the example, `1`.
- **operator**: Operator that determines how the conditions are evaluated. It can be `"and"` or `"or"`. In the example, `"or"`.
- **conditions**: List of conditions that must be met for a card to be ignored.
  - **condition**: Type of condition. It can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field to which the condition applies. It can be any common card field like "type" or "id", or any game-specific fields added in the "metadata" section. Example: `"type"`.
  - **value**: Value the field must have. Example: `"flood"`.
  - **value**: Value the field must have. Example: `"flame"`.


### 8. Layouts

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

- **type**: Defines the type of design. It can be `"border"` or `"background"`. In the example, `"border"` indicates it is a border.
- **selectedCards**: Indicates if the configuration applies to selected cards. In the example, `true`.
- **dialogCards**: Indicates if the configuration applies to the card selection dialog. In the example, `true`.
- **darkTheme**: Indicates if the configuration applies in dark theme. In the example, `true`.
- **lightTheme**: Indicates if the configuration applies in light theme. In the example, `true`.
- **options**: List of design options based on conditions.

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

- **operator**: Operator that determines how conditions are evaluated. It can be `"and"` or `"or"`. Default is `"and"`. In the example, `"and"`.
- **conditions**: List of conditions that must be met to apply this option.
  - **condition**: Type of condition. It can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field to which the condition applies. It can be any common card field like "id" or "points", or any game-specific fields added in the "metadata" section. Example: `"cardType"`.
  - **value**: Value the field must have. Example: `"purple_prosperity"`.
- **color**: Color in ARGB format. The first two hexadecimal digits represent the alpha value. Example: `"ff5f39a5"`.
- **borderWidth**: Border width in pixels. Only applies if `type` is `"border"`. In the example, `10`.
- **borderSide**: Side of the border where the color is applied. Only applies if `type` is `"border"`. Possible values are `"left"`, `"right"`, `"top"`, and `"bottom"`. In the example, `"left"`.
