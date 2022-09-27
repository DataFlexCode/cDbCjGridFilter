Class package: cDbCjGridFilter.pkg, including classes cDbCjGridFilter itself,
cFilterCombo, cFilterForm and cFilterButton.

Provides a mechanism for a cDbCJGrid to be filtered on any database column
present in the grid (apart from Text, Binary or Overlap columns).  It will
skip columns such as Row Indicators or calculated columns. It will also skip
any columns that do not have their psCaption set, as that is what it fills
the "Filter On" combo with.  In fact it will only pay attention to columns of
the class cDbCJGridColumn or cDbCJGridColumnSuggestion.

It assumes that those table values will be in buffer during the Find process,
so if you are populating some buffer in OnPostFind for the main table, or
something like that, it probably won't work on that.
 
Usage: Use it just as you would a cDbCJgrid.  The Grid itself will be
       positioned 15 pixels lower and be 15 pixels shorter than will be
       visible in the Studio designer to make room for two combo forms,
       one form and one button in a row above it.

       Then in the Server DataDictionary object for the grid, add into its 
       Procedure OnConstrain (creating that if there isn't alreadty one) the
       line:

           Constrain {DD'sTable} as (MeetsFilter({gridObjectName}(Self)))

       The first combo form will list the available columns from the grid
       to filter on.

       The second combo will show the filter modes: equals, starts with, 
       contains, greater than and less than.  (In my view that's all the
       user will need, but if you feel defferently then the code is all here
       for you to tweak to your heart's content! <g>)

       The form will allow the entry of a value to compare selected column
       to.

       No filtering will occur until all three contain non-blank values.

       The button simply clears the other three, returning the grid to an
       unfiltered state.

       Only four new properties are exposed in the Studio (cDbCJGrid has
       *quite* enough already, IMHO!), as follows:

       The pbCaseInsensitive property (true by default) determines whether
       comparisons will be case sensitive (therefor not, by default).

       The property pbFilterOnValueChange (true by default) will determine
       whether the list is filtered on each character entered in the value
       form (more intuitive for the user, seeing the list change as they
       type), or only when the form is exited (much more efficient and
       faster).  Ye pays yer money and ye takes yer choice! <g>

       The two properties piFilterColWidth and piFilterValWidth (both default
       to 80) can be used adjust the width of those two controls, as
       required.

The filtering is done totally in DataFlex code, so doesn't care about your
database back-end.  As far as I can tell it is compatible with other
constraints and using SQL filters, although I have not tested that aspect
extensively.

 Author: Mike Peat, Unicorn InterGlobal
