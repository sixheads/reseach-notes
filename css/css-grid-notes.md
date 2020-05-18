# CSS Grid - Wes Bos

- **grid-auto-rows** property will set the size of the implicit rows that follows those you have explicitly set
- grid-auto-columns doesn't really work as they just wrap
- but if you set **grid-auto-flow: column;** it will create implicit columns if you add more than the columns stated (the default it rows)
- with the above set the grid-auto-columns starts to work

## Sizing tracks in CSS Grid
- when using % sizing we run out of space as the grid gap is added on top of the % set
- with the fr unit the grid takes into account the grid gap then works out the space left over - same way it would work with px units on other rows/columns
- auto will take into account the widest piece of content

## Repeat
- you can mix and match repeat with other values
- you can also add multiple units within the repeat:
  grid-template-columns: repeat(4, 1fr 2fr);
- this results in 8 columns repeating those 2 units

## Sizing Grid Items
- we can set a width on an item, this will force that item and any other in that row/column to take up that space, the rest (with 1fr) will distribute the space evenly
- setting **grid-column: span 2;** will let that item span two columns and push the following items onto the next columns
- grid-row will let you span rows
- spaning beyond the space you have will force additional row/col on the grid
- it can result in gaps in your grid if it runs out of space moving down to the next row/col

## Placing Grid Items
- grid-column lets you place an item on a particular track
- its shorthand for grid-column-start and grid-column-end
```css
.item {
  grid-column-start: 3;
  grid-column-end: 5;
  /* or */
  grid-column: 3/5;
}
```
- you can also grid-column: 3 / span 2;
- this would start and track 2 and span 2 columns
- to span across the whole grid:
  grid-column: 1 / -1;
