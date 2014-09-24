---
title: BoundColumnBase
page_title: BoundColumnBase
description: BoundColumnBase
slug: radgridview-columns-column-types-boundcolumnbase
tags: boundcolumnbase
published: True
position: 1
---

# BoundColumnBase



## 

The __GridViewBoundColumnBase__ derives from the __GridViewColumn__, which means that it inherits all of the functionality too. In addition it allows you to easily bind data, format it and edit it using editors. When the columns of the __RadGridView__ are auto generated they are of this type.

Here is a list of the most important properties and methods.

* __DataFormatString__ - allows you to specify formatting for the data displayed in the column via a format string. More about displaying data in the RadGridView [here]({%slug gridview-columns-data-formatting%}).
			

* __DataMemberBinding__ - allows you to specify the binding to the property, whose value you want to display in the column. More about displaying data in the RadGridView [here]({%slug gridview-data-overview%}).
			

* __EditorStyle__ - allows you to specify a __Style__ for the editor of the column.
			

* __EditTriggers__ - allows you to specify what action will set the cell in edit mode. The available options are: __CellClick__, __F2__, __None__, etc.
			

* __FilteringControlStyle__ - allows you to specify a __Style__ for the filtering control.
			

* __IsAutogenerated__ - indicates whether the column is auto generated or not.
			

* __IsReadOnlyBinding__ - defines which cells of the column should be read only. Read more [here]({%slug gridview-read-only-rows-cells%}).
			

* __GetValueForItem(item)__ - a method which returns the content of the cell for the provided item.
			

The following example iterates all cells in all visible columns of the gridview and accesses their values:

#### __C#__

{{region radgridview-columns-column-types-boundcolumnbase_0}}
	var visibleColumns = RadGridView1.Columns.OfType<GridViewBoundColumnBase>()
	                     .Where(c => c.IsVisible)
	                     .OrderBy(c => c.DisplayIndex).ToList();
	for (var i = 0; i < RadGridView1.Items.Count; i++)
	{
	  for (var j = 0; j < visibleColumns.Count; j++)
	  {
	     var value = visibleColumns[j].GetValueForItem(RadGridView1.Items[i]);
	  }
	}
	{{endregion}}



