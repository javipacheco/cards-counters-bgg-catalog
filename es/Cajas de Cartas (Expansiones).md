Contiene la información sobre las diferentes expansiones de cartas disponibles en el juego. El apartado `"boxes"` contiene las cajas del juego. En muchos casos habrá sólo una, la caja "base", pero en otras se podría añadir las cartas de las expansiones. Cada caja contendrá el nombre de la caja y las cartas que trae. 

**Ejemplo en JSON:**
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

- **name**: Nombre de la expansión. En el ejemplo, `"base"`.
- **cards**: Lista de cartas incluidas en esta caja.