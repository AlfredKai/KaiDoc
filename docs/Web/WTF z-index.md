# WTF z-index

[z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index)

[The stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

- The `z-index` CSS property sets the z-order of a ***positioned*** element and its descendants or flex items.
- 原則：比較z-index前要先找到共同的stacking context

## Why can't an element with a z-index value cover its child?

[Why can't an element with a z-index value cover its child?](https://stackoverflow.com/questions/54897916/why-cant-an-element-with-a-z-index-value-cover-its-child/54903621#54903621)

>Stacking contexts formed by positioned descendants with z-indices greater than or equal to 1 in z-index order (smallest first) then tree order.

positioned element設了一個`正`zIndex會創一個新的stacking context，設`負`的不會，所以當B是A的descent的時候，B設了一個zIndex不管比A小多少，只要他是正的他是疊加在A上面的新stacking context，所以A不管設多大也沒用。

## will-change

[will-change](https://css-tricks.com/almanac/properties/w/will-change/)

這雷包有stacking context
