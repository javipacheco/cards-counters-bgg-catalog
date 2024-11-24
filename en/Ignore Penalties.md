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
  - `"valueOfCondition"`: ignores the value of a condition in a penalty. Example below
- **numberOfCards**: Number of cards the user must select if the type is `"some"`. In the example, `1`.
- **valueToIgnore**: Word to ignore in case the type is `"valueOfCondition"`.
- **operator**: Operator that determines how the conditions are evaluated. Can be `"and"` or `"or"`. In the example, `"or"`.
- **conditions**: more information [here](en/Conditions.md)

**Example in JSON for `valueOfCondition`:**

This example ignores the word `army` for all penalties except `dirigible_guerra`.

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
