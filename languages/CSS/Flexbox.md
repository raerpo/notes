# Flexbox
- Applying `display:flex` to an element will convert it into a __Flex container__ and its direct children into __Flex items__.
- By default all flex items align side by side all in one row from left to right.
- A flex container will expand to fill the available width
- the height of the items will all be the same size determine by their content
- flex have to directions __main__ and __cross__. The main axis by default is horizontal from left to right (main-start to main-end) and the cross axis is the vertical one by default from top to bottom (cross-start to cross-end).
- The axis directions can be changed by setting a couple properties
- Flex allows to use the remaining space by using `margin: auto`
- To provide width and height to a flex item you could use `width`, `height` but flex provides better alternatives to determine the dimensions of an item.
- the __flex__ property controls de size of flex items
- When creating layouts with flexbox you need to plan how the website will look on different circustances and not only with the content that you have at the moment.
- the flex propery is shorthand for three properties. `flex-grow`, `flex-shrink` and `flex-basis`.
- the default value for `flex-shrink` is 1 and the default value for `flex-basis` is 0%. So flex: 2 is equivalent to `flex: 2 1 0%`.

## Flex basis
- The flex-basis defines the starting point of an item size
- flex basis can receive the same units as width
- The default value of `flex-basis` is auto so it means that it will look if the element has a defined width and if not it will be set to the content.
- If a `flex-basis` is defined it will overwrite the elements setted width.
- Once the initial width is set on a item, it may need to grow or shrink to fit the container.

## Flex grow
- After the width setted by the basis is calculated (including possible margins), there could be possible remaining spaces. The remaining space will be consumed based on the flex-grow property.
- flex grow can only have non-negative integers
- If any item have a non-zero grow value, it will grow until filling the remaining space
- a higher number in grow will add more "weight" in the space repartion. So if a have an element with grow as 2 and another one with a grow of 1. The remaining space will be divided into 3 pieces. The first one will take 2/3 of the remaining space and the last one will get 1/3 of it.
- When we set a property like `flex: 2` and `flex: 1` to another element. It means that flex-basis is set to 0% and that means that 100% of the space (minus any margin between them) is up for grabs. So the first one will take 2/3 of the entire width and the last one will take 1/3 of the width. __Actually the remaining space is not 100% because the padding of the items is taken in consideration when basis is 0%. En reality when basis is 0% the remaining space is calc(100% - horizontal padding of the items). To avoid this behaviour we can use the method used on the shrink section (**)__
- You should prefer the use of the `flex` shorthand since is different that other shorthand declarations that set the unsetted values to their initial values. Instead `flex` will give useful default values. 1,1,0%.

## Flex shrink
- After determining the initial size based on the basis value it can be the case when the items overflow the container. For example when we set the basis of two elements to 60% each.
- The flex shrink value of a item indicates if an item should shrink to avoid overflowing.
- A `flex-shrink` of 0 indicates that the item will not shrink
- values greater than 0 will shrink until there's no overflow
- a higher shrink value indicates that the item will shrink more than others
- (**) We can also create a layout by using flex-shrink. If we set the first basis to 66.67% and the second one to 33.33% we will overflow the layout by having a margin between them. If we set the property `flex-shrink` to 1 for both of those elements each one of them will shrink the same to avoid overflowing.
