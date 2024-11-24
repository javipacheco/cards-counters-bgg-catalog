This is the most important part of the file, as it contains information about each of the cards. Each card will have some common fields like an identifier or the points of that card, but it will also have a series of metadata that is exclusive information for the game being built. For example, `Everdell` has the number of resins that the card is worth, and this information is exclusive to `Everdell`. Additionally, we must add information about the bonuses the card gives or if it cancels or ignores other cards in your hand. 

To understand the fields of the cards it is essential to understand how the process that loads the information of each card is executed. For each card there are 3 very important parts: 

- **Bonus**: it is any extra bonus for the card, for that it must fulfill some conditions normally.
- **Penalties**: can be for example negative bonuses (for a condition you lose points) or cancels other cards (for example remove the positive bonus of another card).
- **Ignore penalties:** Cards can make you ignore the penalty of other things, usually by conditions.

Each field that a card can contain will be explained below, but the first thing to know is the order of execution:

1. Cards that ignore penalties are calculated.
2. The penalties are calculated, taking into account whether or not they have been previously ignored.
3. The bonus of the card that can be eliminated by the penalty of another card, for example, is calculated to be 0.

Each card has the following structure:

1. [[Common fields]]
2. [[Metadata]]
3. [[en/Bonus|Bonus]]
	1. Contar Cartas
	2. En Colección
	3. Contar Otros Ítems
	4. Combinar diferentes bonus
4. [[Blanks]]
	1. Anular otras cartas
	2. Anula la propia carta a menos que tenga cartas
	3. Anula la propia carta si tiene cartas