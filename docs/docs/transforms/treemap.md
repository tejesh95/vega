---
layout: transform
title: Treemap Transform
permalink: /docs/transforms/treemap/index.html
---

The **treemap** transform recursively subdivides area into rectangles with areas proportional to each node's associated value.

Internally, this transform processes a collection of special tree node objects generated by an upstream [nest](../nest) or [stratify](../stratify) transform. The original input data object can be accessed under the `data` field of these tree node objects. This transform uses the [d3-hierarchy library](https://github.com/d3/d3-hierarchy).

## Example

{% include embed spec="treemap" %}

## Transform Parameters

| Property            | Type                           | Description   |
| :------------------ | :----------------------------: | :------------ |
| field               | {% include type t="Field" %}   | The data field corresponding to a numeric value for the node. The sum of values for a node and all its descendants is available on the node object as the `value` property. This field determines the size of a node.|
| sort                | {% include type t="Compare" %} | A comparator for sorting sibling nodes. The inputs to the comparator are tree node objects, not input data objects.|
| method              | {% include type t="String" %}  | The layout method to use. One of `squarify` (the default), `resquarify`, `binary`, `dice`, `slice`, or `slicedice`. The `resquarify` method will preserve relative node positions once an initial layout has been computed, even if node sizes change.|
| padding             | {% include type t="Number" %}  | Sets both the _paddingInner_ and _paddingOuter_ parameters to the same value.|
| paddingInner        | {% include type t="Number" %}  | The padding used to separate a node's adjacent children (default `0`).|
| paddingOuter        | {% include type t="Number" %}  | The padding used to separate the edges of a parent node from its contained children (default `0`). Sets the _paddingTop_, _paddingRight_, _paddingBottom_ and _paddingLeft_ parameters to the same value.|
| paddingTop          | {% include type t="Number" %}  | The padding used to separate the top edge of a node from its children.|
| paddingRight        | {% include type t="Number" %}  | The padding used to separate the right edge of a node from its children.|
| paddingBottom       | {% include type t="Number" %}  | The padding used to separate the bottom edge of a node from its children.|
| paddingLeft         | {% include type t="Number" %}  | The padding used to separate the left edge of a node from its children.|
| ratio               | {% include type t="Number" %}  | The target aspect ratio for the `squarify` or `resquarify` methods. The default is the golden ratio, φ = (1 + sqrt(5)) / 2.|
| round               | {% include type t="Boolean" %} | Indicates if node layout values should be rounded (default `false`).|
| size                | {% include type t="Number[]" %}| The size of the layout, provided as a [width, height] array.|
| as                  | {% include type t="String[]" %}| The output fields at which to write the layout results. The default is `["x0", "y0", "x1", "y1", "depth", "children", "value"]`, where `x0`, `y0`, `x1` and `y1` are the starting and ending layout coordinates for each segment, `depth` is the tree depth, `children` is the count of a node's children in the tree, and `value` is the sum of values for a node and all its descendants.|

## Usage

```json
{
  "type": "treemap",
  "field": "value",
  "method": "resquarify",
  "ratio": 1,
  "paddingInner": 0,
  "paddingOuter": 2,
  "size": [{"signal": "width"}, {"signal": "height"}]
}
```

Computes a treemap layout using a stable squarified method (`resquarify`), a square aspect ratio (`1`), and `2` pixels of outer padding between parent and child nodes. The layout is sized to use the full width and height of the view.
