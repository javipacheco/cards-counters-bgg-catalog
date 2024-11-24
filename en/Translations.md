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

