Esta es la parte más importante del archivo, ya que contiene la información de cada una de las cartas. Cada carta tendrá unos campos comunes como un identificador o los puntos de esa carta, pero también tendrá una serie de metadatos que es información exclusiva para el juego que se está construyendo, por ejemplo `Everdell` tiene un el número de resinas que vale la carta y esta información es exclusiva para `Everdell`. Además debemos añadir información de los bonus que da la carta o si anula o ignora otras cartas de tu mano. 

Para entender los campos de las cartas es imprescindible entender como se ejecuta el proceso que carga la información de cada. Para cada carta hay 3 partes muy importantes que son: 

- **Bonus**: es cualquier bonificación extra para la carta, para ello debe completar una condiciones normalmente
- **Penalizaciones**: pueden ser por ejemplo bonus negativos (por una condición se pierden puntos) o anular otras cartas (por ejemplo elimina el bonus positivo de otra carta)
- **Ignorar penalizaciones**: Cartas pueden hacer que ignore la penalización de otras cosas, normalmente mediante condiciones

A continuación se explicará cada campo que puede contener una carta, pero lo primero es saber el orden de ejecución:

1. Se calculan las cartas que ignoran penalizaciones
2. Se calculan las penalizaciones, teniendo en cuenta si han sido ignoradas o no previamente
3. Se calcula el bonus de la carta que puede eliminarse por la penalización de otra carta por ejemplo y ser 0

Cada carta tiene la siguiente estructura:

1. [[Campos comunes]]
2. [[Metadatos]]
3. [[Bonus]]
	1. Contar Cartas
	2. En Colección
	3. Contar Otros Ítems
	4. Combinar diferentes bonus
4. [[Anular]]
	1. Anular otras cartas
	2. Anula la propia carta a menos que tenga cartas
	3. Anula la propia carta si tiene cartas
5. [[Ignorar Penalizaciones]]