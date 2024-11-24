Contiene las traducciones de mensajes y nombres de cartas en diferentes idiomas. Los idiomas soportados por el momento son:

- Ingles (en)
- Español (es)
- Alemán (de)
- Portugués (pt)
- Italiano (it)
- Polaco (pl)
- Ruso (ru)

#### a. Mensajes

Estos mensaje se podrán usar en algunos bonus o campos de las cartas.

**Ejemplo en JSON:**
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

- **translations.others**: Son textos generales, donde se usará la llave, por ejemplo `arquitecto_msg` y será traducido al idioma correspodiente.

#### b. Cartas

Estas son todas las traducciones de las cartas por tu identificador.

**Ejemplo en JSON:**
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

- **translations.cards**: Traducciones de los nombres de las cartas.
  - **[id_de_carta]**: Traducción del nombre de la carta.
    - **es**: Nombre en español. Ejemplo: `"Árbol Eterno"`
    - **en**: Nombre en inglés. Ejemplo: `"Ever Tree"`