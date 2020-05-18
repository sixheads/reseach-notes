# What the Flexbox - Wes Bos

- we have a flex container wrapping out elements
- and flex items within our container

- define flex with: display: flex;
- flex is block level - spans full width
- also have inline-flex

## Working with Flexbox flex-direction
- default direction is : flex-direction: row;
- also have: column, row-reverse & column-reverse
- we have a main and cross axis when using flexbox
- row: main access left to right, cross top to bottom

## Wrapping elements with Flexbox
- when setting a width on flex items flexbox will try to match the width but if it can't it will distribute the space evenly
- to use wrap we apply a flex-wrap on the container
- default is nowrap
- applying wrap will mean flexbox will match your width and then wrap items down to the next row if they don't fit
- also have wrap-reverse

## Flexbox Ordering
- we apply order on our flex items
- default value is 0
- apply order: 2; to change the order of an item
- you can also apply a negative value

## Flexbox Alignment and Centering with justify-content
- justify-content goes on the container
- it works on the main axis
- values include: flex-end, flex-start, space-between, space-around
- if you change to the flex direction to column you will need to set the height of the container for justify-content to work

## Alignment and Centering with align-items
- align-items aligns items along the cross axis
- it gets applied to the container
- your container needs a height
- by default it is set to stretch
- values: stretch, flex-start, flex-end, center, baseline
- baseline aligns the actual baseline of text in your items

## Alignment and Centering with align-content
- align-content works on the cross axis
- you do need multiple lines of items for this to work - so you need to apply a **flex-wrap: wrap;**
- by default it is set to stretch
- values include: stretch, flex-start, flex-end, center, space-between, space-around

## Alignment and Centering with align-self
- align-self is applied to an item - gives you control over each separately
- it will override the align-content set on the container

## Understanding Flexbox sizing with the flex property
- applying flex: 1;  to all flex items will make them all use the space left over proportionally - they will stretch to fit the space
- basically how you use the extra space
- applying flex: 2;  to one of the items will make that take up twice the space of the other items proportionally

## Finally understanding Flexbox flex-grow, flex-shrink and flex-basis
- the flex property is shorthand for:
  - flex-grow: how to use the extra space available
  - flex-shrink: how to shrink an item when there is no extra room
  - flex-basis: the ideal size if space available - eg. 300px
- to write those in shorthand we can do. flex: 1 2 300px;
- it follows the order of grow, shrink, basis

## How Flexbox's flex-basis and wrapping work together
- flex properties only work on the row they are on when wrapping with flexbox
- setting flex-basis: 500px; then applying wrap will wrap across multiple lines
- add a flex-grow: 1; will fill the extra space


