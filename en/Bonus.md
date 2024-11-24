Bonuses are calculated based on certain specified conditions. Each type of bonus will help you count the extra points of the cards according to their type.

There is a field in the bonus that is common to all types of bonuses which is `affectCards`. It can have different cases:

- **allExceptMe**: To calculate the bonus all cards in your hand are used, except the card itself that is calculating the bonus value. This is the default value
- **all**: To calculate the bonus all cards are used, including the card that is calculating the bonus.

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
  - **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. In the example, `"or"`.
  - **conditions**: more information [here](en/Conditions.md)
  - **multiply**: Multiplier for the points. In the example, `1`.

Each entry in the **countCards** list represents a different set of conditions and a corresponding multiplier for the points.

##### 2. In Collection

This type of bonus adds extra points to the card in question if you have other cards in your hand. For example, in the following example for "Fantasy Realms", you will earn 50 points if you have the card with the identifier `fire` or `smoke` in your hand.

**Example in JSON:**
```json
{
    "bonus": {
        "inCollection": {
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
  - **strategy**: Strategy for evaluating the conditions. It can be:
        - `"oneCardFulfillSomeCondition"` (default) where one card must fulfill some condition
        - `"oneCardFulfillAllTheConditions"` where one card must fulfill all the conditions
        - `"eachConditionIsFulfilledByACard"` where each condition must be fulfilled by at least one different card
        - `"eachConditionIsFulfilledByAllCards"` where each condition must be fulfilled by all cards
        - `"unlessOneCardFulfillAllTheConditions"` unless you have one card that fulfills all the conditions
  - **conditions**: more information [here](en/Conditions.md)
  - **points**: Additional points awarded if the specified conditions are met. In the example, `50`.

##### 3. Count Other Items

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

##### 4. Combine different bonuses

This type of bonus combines different bonuses to choose one of them. For example, you can have
different options and choose which of them is the best (gives you more points).

Within these options you can find 2 lists:

1. `countCardsOptions`: a list of “Count Cards” bonus (described in section “5.c.1”)
2. `inCollectionOptions`: a bonus list “In Collection” (described in section “5.c.2”)

For the following example we have 2 options:

1. You earn 15 points if you have a card with `type` equal to `leader`.
2. You earn 40 points if, in addition to having a card with `type` equal to `leader`, you also have the card with the identifier `keth_sword`.

Out of the 2 cases, this bonus will add the one with the highest score, as in the example the `type` is equal to `best`.

**Example in JSON:**
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

The new fields are:

- **type**: Defines how the best option from the given conditions will be selected. It can be:
  - `"best"`: Selects the option with the highest score.
  - `"worst"`: Selects the option with the lowest score.
  - `"first"`: Selects the first option in order that meets the condition.

In the example, the type is `"best"`, meaning that the option with the highest score among those that meet the conditions will be selected.
