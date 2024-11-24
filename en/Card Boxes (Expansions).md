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

