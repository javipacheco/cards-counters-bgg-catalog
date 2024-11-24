The list of conditions is used in many areas to obtain bonuses, requirements, cancel, or ignore cards. Depending on where it is applied, it will serve a different purpose. For example, for a bonus that looks for cards in your hand to multiply each card by 5, these would be the conditions it needs to search for.

Here’s an example where it looks for cards with the identifiers `fire` and `smoke`.

```json
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
```

The `conditions` node contains a list with 3 properties:

- **condition**: Type of condition. Can be `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Example: `"equal"`.
- **field**: Field on which the condition is applied. Can be any common field of the card like "id" or "points", or any of the game-specific fields added in the "metadata" section. Example: `"cardType"`.
- **value**: Value the field must have. Example: `"purple_prosperity"`.