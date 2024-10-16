# pandas-challenge
# I recieved the following error on the last task-
# Group the per_school_summary DataFrame by "School Type" and average the results:
KeyError                                  Traceback (most recent call last)
Cell In[81], line 4
      1 # Group the per_school_summary DataFrame by "School Type" and average the results.
----> 4 average_math_score_by_type = per_school_summary.groupby(["School Type"])["Average Math Score"].mean()
      5 average_reading_score_by_type = per_school_summary.groupby(["School Type"])["Average Reading Score"].mean()
      6 average_percent_passing_math_by_type = per_school_summary.groupby(["School Type"])["% Passing Math"].mean()

File /opt/anaconda3/envs/dev/lib/python3.10/site-packages/pandas/core/frame.py:9183, in DataFrame.groupby(self, by, axis, level, as_index, sort, group_keys, observed, dropna)
   9180 if level is None and by is None:
   9181     raise TypeError("You have to supply one of 'by' and 'level'")
-> 9183 return DataFrameGroupBy(
   9184     obj=self,
   9185     keys=by,
   9186     axis=axis,
   9187     level=level,
   9188     as_index=as_index,
   9189     sort=sort,
   9190     group_keys=group_keys,
   9191     observed=observed,
   9192     dropna=dropna,
   9193 )

File /opt/anaconda3/envs/dev/lib/python3.10/site-packages/pandas/core/groupby/groupby.py:1329, in GroupBy.__init__(self, obj, keys, axis, level, grouper, exclusions, selection, as_index, sort, group_keys, observed, dropna)
   1326 self.dropna = dropna
   1328 if grouper is None:
-> 1329     grouper, exclusions, obj = get_grouper(
   1330         obj,
   1331         keys,
   1332         axis=axis,
   1333         level=level,
   1334         sort=sort,
   1335         observed=False if observed is lib.no_default else observed,
   1336         dropna=self.dropna,
   1337     )
   1339 if observed is lib.no_default:
   1340     if any(ping._passed_categorical for ping in grouper.groupings):

File /opt/anaconda3/envs/dev/lib/python3.10/site-packages/pandas/core/groupby/grouper.py:1043, in get_grouper(obj, key, axis, level, sort, observed, validate, dropna)
   1041         in_axis, level, gpr = False, gpr, None
   1042     else:
-> 1043         raise KeyError(gpr)
   1044 elif isinstance(gpr, Grouper) and gpr.key is not None:
   1045     # Add key to exclusions
   1046     exclusions.add(gpr.key)

KeyError: 'School Type'

# To fix this I submitted the following code:
if "School Type" not in per_school_summary.columns:
    per_school_summary = pd.merge(per_school_summary, school_data[["school_name", "type"]], left_index=True, right_on="school_name")
    per_school_summary.rename(columns={"type": "School Type"}, inplace=True)

This error occurred because it appears the per_school_summary DataFrame did not have a column named "School Type", which led to the Key Error when trying to group the data by this column. 
First, I added a condition to check if the School Type column is missing from per_school_summary. If it was missing, the code added it.
Next, I merged the "school_name" and "type" columns from school_data into per_school_summary since "School Type" column exists in the original school_data Dataframe which had information about school type (charter or district).
Lastly after merging, I renamed the "type" column from school_data to School Type to match the column needed for this task.
Once School Type column was included, the code worked.

References:
https://realpython.com/pandas-merge-join-and-concat/
https://www.programiz.com/python-programming/pandas/merge
https://jakevdp.github.io/PythonDataScienceHandbook/03.07-merge-and-join.html
