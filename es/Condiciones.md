La lista de condiciones se usa en muchas partes para obtener los bonus, requerimientos, anular o ignoras cartas. Según el lugar donde se use servirá para una cosa u otra. Por ejemplo, para un bonus donde busca cartas de tu mano para multiplicar por 5 cada carta, estas serían las condiciones que tiene que buscar.

A continuación un ejemplo donde busca las cartas con identificar `incendio` y `humo`

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

El nodo `conditions` contiene una lista con 3 propiedades:

- **condition**: Tipo de condición. Puede ser `"equal"`, `"notEqual"`, `"greater"`, `"less"`, `"greaterOrEqual"`, `"lessOrEqual"`. Ejemplo: `"equal"`.
- **field**: Campo sobre el cual se aplica la condición. Puede ser cualquier campo común de la carta como `id`, `type`o `points`, o cualquiera de los campos específicos del juego añadidos en el apartado de "metadata". Ejemplo: `"cardType"`.
- **value**: Valor que debe tener el campo. Ejemplo: `"purple_prosperity"`.
