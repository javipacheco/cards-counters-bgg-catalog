The "blanks" section contains information on how to blank the properties of other or the cards itself. A card with blanks will apply these conditions to the cards you have played, making them lose their type, base points, bonuses, and penalties. Below is the structure and fields of this section.

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

##### 1. Blanking Other Cards

Default strategy. Cancels other cards in your hand

**Example in JSON:**
```json
{
    "strategy": "otherCards",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "army"
        }
    ]
}
```

- **conditions**: more information [here](en/Conditions.md)

##### 2. Blanking myselft at least

Cancels own card unless it has cards that meet the condition

**Ejemplo en JSON:**
```json
{
    "strategy": "myselfAtLeast",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "flame"
        }
    ]
}
```

##### 3. Blanking myself with

Cancels own card if it has cards that meet the condition

**Ejemplo en JSON:**
```json
{
    "strategy": "myselfWith",
    "conditions": [
        {
            "condition": "equal",
            "field": "type",
            "value": "flood"
        }
    ]
}
```
