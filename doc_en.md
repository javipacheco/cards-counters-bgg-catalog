
# Manual for Generating a Scoring System for Board Games

This manual provides a detailed guide on how to generate a scoring system for board games using a JSON file. Below is the structure of the JSON file with specific examples for the games "Everdell" and "Fantasy Realms."

## JSON Structure

### 1. Game Information

The first step is to name the game.

**Example in JSON:**
```json
{
    "game": "Everdell",
    "maxCards": null
}
```

- **game**: Name of the game. In the example, `"Everdell"`.
- **maxCards**: Maximum number of cards allowed. Can be `null` if there is no specific limit. In the example, `null`.

### 2. Card Boxes (Expansions)

Contains information about the different card expansions available in the game. The `"boxes"` section contains the game boxes. In many cases, there will only be one, the "base" box, but in others, you could add cards from expansions. Each box will contain the name of the box and the cards it includes.

**Example in JSON:**
```json
{
    "boxes": [
        {
            "name": "base",
            "cards": [
                {
                    "id": "eternal_tree",
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
- **cards**: List of cards included in this box.


### 3. Cards

This is the most important part of the file, as it contains information about each of the cards. Each card will have some common fields like an identifier or the points of that card, but it will also have a series of metadata that is exclusive information for the game being built. For example, "Everdell" has the number of resins that the card is worth, and this information is exclusive to "Everdell." Additionally, we must add information about the bonuses the card gives or if it cancels or ignores other cards in your hand. Everything is explained below.

Each card has the following structure:

#### a. Common Fields

**Example in JSON:**
```json
{
    "id": "eternal_tree",
    "points": 5,
    "type": "construction",
    "unique": true
}
```

- **id**: Unique identifier of the card. In the example, `"eternal_tree"`.
- **points**: Base points of the card. In the example, `5`.
- **type**: Type of card (e.g., "construction", "critter"). In the example, `"construction"`.
- **unique**: Indicates whether the card is unique. In the example, `true`.

#### b. Metadata

Additional game-specific information. You can add the necessary information to later use in the "Bonus" conditions, for example.

This is an example of metadata for "Everdell," but here you can add any information related to the game.

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

For example, in this case, as we will see in the conditions, we can query the cards where `cardType` is equal to `purple_prosperity`. You can imagine this for any field you have in your game.

#### c. Bonus

Bonuses are calculated based on certain specified conditions. Each type of bonus will help you count the extra points of the cards according to their type.

There are 4 types of bonuses:

##### 1. Count Cards

This type of bonus adds points to the card by counting the number of other cards you have in your hand. For example, in this case, for each card you have in your hand that meets the condition where `cardType` is equal to `purple_prosperity`, you will add 1 extra point.

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

- **countCards**: Counts the cards according to the specified conditions and adds extra points according to the multiplier.
  - **conditions**: List of conditions that must be met for the cards to be counted.
    - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
    - **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "points", or any of the game-specific fields added in the "metadata" section. Example: `"cardType"`.
    - **value**: Value the field must have. Example: `"purple_prosperity"`.
  - **multiply**: Multiplier for the points. In the example, `1`.

Each entry in the **countCards** list represents a different set of conditions and a corresponding multiplier for the points.

##### 2. In Collection

This type of bonus adds extra points to the card in question if you have other cards in your hand. For example, in the following example for "Fantasy Realms", you will earn 50 points if you have the card with the identifier `fire` or `smoke` in your hand.

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
                    "value": "fire"
                },
                {
                    "condition": "equal",
                    "field": "id",
                    "value": "smoke"
                }
            ],
            "points": 50
        }
    }
}
```

- **inCollection**: Checks if certain conditions are met for the cards in your collection.
  - **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. Default is `"and"`. In the example, `"or"`.
  - **strategy**: Strategy for evaluating the conditions. Can be `"eachConditionIsFulfilledByACard"` where each condition must be met by at least one card, or `"oneCardFulfillAllTheConditions"` where one card must meet all conditions. Default is `"oneCardFulfillAllTheConditions"`. In the example, `"eachConditionIsFulfilledByACard"`.
  - **conditions**: List of conditions that must be met.
    - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
    - **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "points", or any of the game-specific fields added in the "metadata" section. Example: `"id"`.
    - **value**: Value the field must have. Example: `"fire"`.
    - **value**: Value the field must have. Example: `"smoke"`.
  - **points**: Additional points awarded if the specified conditions are met. In the example, `50`.


##### 3. In Collection with Options

This type of bonus is similar to the "In Collection" bonus, but you will add different options, and it will only give you the highest scoring one. In this case, of the 2 options:

1. You earn 15 points if you have a card with `type` equal to `leader`.
2. You earn 40 points if, in addition to having a card with `type` equal to `leader`, you also have the card with the identifier `keth_sword`.

Out of the 2 cases, this bonus will add the one with the highest score, as in the example the `type` is equal to `best`.

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
                            "value": "keth_sword"
                        }
                    ],
                    "points": 40
                }
            ]
        }
    }
}
```

Each option within **inCollectionWithOptions** is the same as the conditions described in section 5.b "In Collection."

- **type**: Defines how the best option from the given conditions will be selected. It can be:
  - `"best"`: Selects the option with the highest score.
  - `"worst"`: Selects the option with the lowest score.
  - `"first"`: Selects the first option in order that meets the condition.

In the example, the type is `"best"`, meaning that the option with the highest score among those that meet the conditions will be selected.

##### 4. Count Other Items

This is typically used to count points at the end of the game based on the number of resources you have, objectives achieved, etc. This data must be manually added by the player and cannot be automatically calculated. For example, in this case in "Everdell," the user has to count the resources they have at the end of the game and add them, which will be the points to be added as a bonus, with a maximum of 6.

**Example in JSON:**
```json
{
    "bonus": {
        "countOtherItems": [
            {
                "translation": "architect_msg",
                "max": 6,
                "multiply": 1
            }
        ]
    }
}
```

- **translation**: Translation message obtained from `translations.others`. In the example, `"architect_msg"`.
- **max**: Maximum number of items to count. In the example, `6`.
- **multiply**: Multiplication factor for the points. In the example, `1`.

#### d. Blanks

The "blanks" section contains information on how to blank the properties of other cards. A card with blanks will apply these conditions to the cards you have played, making them lose their type, base points, bonuses, and penalties. Below is the structure and fields of this section.

In this case, for the game "Fantasy Realms," if you have this card in hand, it blanks all cards where `type` is `army`, `leader`, and `beast` (in the latter case except for the card with identifier `basilisk`). For all those cards, base points, bonuses, and penalties will not be counted.

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

Below are the fields in the "Blanks" section:

##### 1. List of Conditions for Blanking Other Cards

Each entry within "blanks" has the following structure:

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
  - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "type", or any of the game-specific fields added in the "metadata" section. Example: `"type"`.
  - **value**: Value the field must have. Example: `"army"`.

##### 2. Combined Conditions

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

- **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. Default is `"and"`. In the example, `"and"`.
- **conditions**: List of conditions that must be met for a card to be blanked.
  - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "type", or any of the game-specific fields added in the "metadata" section. Example: `"type"`.
  - **value**: Value the field must have. Example: `"beast"`.
  - **condition**: Type of condition. Example: `"notEqual"`.
  - **field**: Field on which the condition is applied. Example: `"id"`.
  - **value**: Value the field must have. Example: `"basilisk"`.


#### e. Ignore Penalties

The "clears" section allows specifying cards whose penalties will be ignored. All ignored actions occur before the penalty is applied.

In the following case from the game "Fantasy Realms," the user can select a card to ignore its penalties, but they cannot choose just any card. The card they can select is within the conditions, in this case, where the `type` is `flood` or `flame`. The application will enable a button for the user to select the card.

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

Below are the fields in the "Ignore Penalties" section:

- **type**: Defines how the conditions will be applied. It can be:
  - `"all"`: Ignores all values of the cards that meet the conditions.
  - `"some"`: The user must select 1 or more cards from those that meet the conditions.
- **numberOfCards**: Number of cards the user must select if the type is `"some"`. In the example, `1`.
- **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. In the example, `"or"`.
- **conditions**: List of conditions that must be met for a card to be ignored.
  - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field on which the condition is applied. Can be any common field of the card like "type" or "id", or any of the game-specific fields added in the "metadata" section. Example: `"type"`.
  - **value**: Value the field must have. Example: `"flood"`.
  - **value**: Value the field must have. Example: `"flame"`.

### 4. Translations

Contains the translations of messages and card names in different languages. The languages currently supported are:

- English (en)
- Spanish (es)
- German (de)
- Portuguese (pt)
- Italian (it)
- Polish (pl)
- Russian (ru)

#### a. Messages

These messages can be used in some bonuses or card fields.

**Example in JSON:**
```json
{
    "translations": {
        "others": {
            "architect_msg": {
                "es": "por cada resina y guijarro, máximo 6",
                "en": "for each resin and pebble, maximum 6"
            },
            "king_msg_1": {
                "es": "por cada Evento Básico",
                "en": "for each Basic Event"
            },
            "king_msg_2": {
                "es": "por cada Evento Especial",
                "en": "for each Special Event"
            }
        }
    }
}
```

- **translations.others**: These are general texts, where the key will be used, for example, `architect_msg`, and it will be translated into the corresponding language.

#### b. Cards

These are all the card translations by their identifier.

**Example in JSON:**
```json
{
    "translations": {
        "cards": {
            "eternal_tree": {
                "es": "Árbol Eterno",
                "en": "Ever Tree"
            },
            "wife": {
                "es": "Esposa",
                "en": "Wife"
            }
        }
    }
}
```

- **translations.cards**: Translations of card names.
  - **[card_id]**: Translation of the card name.
    - **es**: Name in Spanish. Example: `"Árbol Eterno"`
    - **en**: Name in English. Example: `"Ever Tree"`


### 5. Layouts

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
- **conditions**: List of conditions that must be met to apply this option.
  - **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
  - **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "points", or any of the game-specific fields added in the "metadata" section. Example: `"cardType"`.
  - **value**: Value the field must have. Example: `"purple_prosperity"`.
- **color**: Color in ARGB format. The first two hexadecimal digits represent the alpha value. Example: `"ff5f39a5"`.
- **borderWidth**: Border width in pixels. Applies only if the `type` is `"border"`. In the example, `10`.
- **borderSide**: Side of the border where the color is applied. Applies only if the `type` is `"border"`. Possible values are `"left"`, `"right"`, `"top"`, and `"bottom"`. In the example, `"left"`.

