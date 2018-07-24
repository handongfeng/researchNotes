1. Use the rename function and refer the columns to be renamed. Not all the columns have to be renamed:
'df = df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'})'
OR
'df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'}, inplace=True)'


2. Pandas find rows in A not in B
https://stackoverflow.com/questions/32652718/pandas-find-rows-which-dont-exist-in-another-dataframe-by-multiple-columns
Since 0.17.0 there is a new indicator param you can pass to merge which will tell you whether the rows are only present in left, right or both:
merged = df.merge(other, how='left', indicator=True)
merged

Out[5]:
   col1 col2  extra_col     _merge
0     0    a       this  left_only
1     1    b         is       both
2     1    c       just  left_only
3     2    b  something  left_only

In [6]:    
merged[merged['_merge']=='left_only']

Out[6]:
   col1 col2  extra_col     _merge
0     0    a       this  left_only
2     1    c       just  left_only
3     2    b  something  left_only
So you can now filter the merged df by selecting only 'left_only' rows
