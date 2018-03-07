

```python
import pandas as pd
import numpy as np
```


```python
#Read csv and put in dataframe
schools_df = pd.read_csv("schools_complete.csv")
students_df = pd.read_csv("students_complete.csv")
#Rename the column name to school in schools_complete.csv
schools_df.rename(columns = {'name': 'school'}, inplace = True)
#merge two dataframes using school names
merged_df = students_df.merge(schools_df, how = 'left', on = 'school')

```


```python
#New array of different school names
unique_school_names = schools_df['school'].unique()

#Total schools
total_schools = len(unique_school_names)

#Total students
total_students = schools_df['size'].sum()

#Total Budget
total_budget = schools_df["budget"].sum()

#Average math score
avg_math = students_df["math_score"].mean()

#Average reading score
avg_reading = students_df["reading_score"].mean()

#Students passing math (greater than or equal to 65)
passing_math_students = students_df.loc[students_df["math_score"] >=65]["math_score"].count()

#Students passing reading (greater than or equal to 65)
passing_reading_students = students_df.loc[students_df["reading_score"] >=65]["reading_score"].count()

#Students passing Math
perc_passing_math = passing_math_students/total_students

#Students passing Reading
perc_passing_reading = passing_reading_students/total_students

#Overall Passing rate
overall_passing = (perc_passing_math+perc_passing_reading)/2


district_summary = pd.DataFrame([[total_schools, total_students, total_budget, avg_math, 
                                  avg_reading, perc_passing_math, perc_passing_reading, overall_passing]],
                               columns=["Total Schools", "Total Students", "Total Budget", "Average Math Score",
                                        "Average Reading Score","% Passing Math", 
                                        "% Passing Reading", "% Overall Passing"])

district_summary = district_summary.style.format({"Total Budget": "${:,.2f}", 
                       "Average Reading Score": "{:.1f}", 
                       "Average Math Score": "{:.1f}", 
                       "% Passing Math": "{:.1%}", 
                       "% Passing Reading": "{:.1%}", 
                       "% Overall Passing": "{:.1%}"})
print("District Summary")
district_summary

```

    District Summary





<style  type="text/css" >
</style>  
<table id="T_ffb12e98_2249_11e8_903c_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Math Score</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >% Passing Math</th> 
        <th class="col_heading level0 col6" >% Passing Reading</th> 
        <th class="col_heading level0 col7" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ffb12e98_2249_11e8_903c_542696dc2a8blevel0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col0" class="data row0 col0" >15</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col1" class="data row0 col1" >39170</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col3" class="data row0 col3" >79.0</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col4" class="data row0 col4" >81.9</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col5" class="data row0 col5" >84.7%</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col6" class="data row0 col6" >96.2%</td> 
        <td id="T_ffb12e98_2249_11e8_903c_542696dc2a8brow0_col7" class="data row0 col7" >90.5%</td> 
    </tr></tbody> 
</table> 




```python
#Set index to School names
school_as_index = merged_df.set_index("school").groupby(["school"])

#Types of schools
school_types = schools_df.set_index("school")["type"]

#students per school
stu_per_sch = school_as_index['Student ID'].count()

#Total school Budget
school_budget = schools_df.set_index("school")["budget"]

#Per student Budget
per_stu_budget = schools_df.set_index("school")["budget"]/schools_df.set_index("school")["size"]

#Average Math Score
avg_mathScore = school_as_index["math_score"].mean()

#Average Reading Score
avg_readScore = school_as_index["reading_score"].mean()

#Passing Math
passing_math = merged_df[merged_df["math_score"] >= 65].groupby("school")["Student ID"].count()/stu_per_sch

#Passing Reading
passing_reading = merged_df[merged_df["reading_score"] >= 65].groupby("school")["Student ID"].count()/stu_per_sch

#Overall Passing rate
overall_passing_bySchool = (passing_math+passing_reading)/2

school_summary = pd.DataFrame({"School Type":school_types, "Total Students":stu_per_sch, 
                               "Total Budget":school_budget,"Per Student Budget":per_stu_budget,
                               "Average Math Score":avg_mathScore,"Average Reading Score":avg_readScore,
                               "% Passing Math":passing_math, "% Passing Reading":passing_reading,
                               "% Overall Passing":overall_passing_bySchool})
#Arrange the columns
school_summary = school_summary[["School Type","Total Students","Total Budget","Per Student Budget",
                                 "Average Math Score","Average Reading Score","% Passing Math",
                                 "% Passing Reading","% Overall Passing"]]

school_summary.style.format({"Total Budget": "${:,}", 
                       "Average Reading Score": "{:.1f}", 
                       "Average Math Score": "{:.1f}", 
                       "% Passing Math": "{:.1%}", 
                       "% Passing Reading": "{:.1%}", 
                       "% Overall Passing": "{:.1%}"})


```




<style  type="text/css" >
</style>  
<table id="T_00378c4a_224a_11e8_9cb3_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col0" class="data row0 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col1" class="data row0 col1" >4976</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col2" class="data row0 col2" >$3,124,928</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col3" class="data row0 col3" >628</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col4" class="data row0 col4" >77.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col5" class="data row0 col5" >81.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col6" class="data row0 col6" >77.9%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col7" class="data row0 col7" >94.6%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow0_col8" class="data row0 col8" >86.2%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col1" class="data row1 col1" >1858</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col2" class="data row1 col2" >$1,081,356</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col3" class="data row1 col3" >582</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col4" class="data row1 col4" >83.1</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col5" class="data row1 col5" >84.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col6" class="data row1 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col7" class="data row1 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow1_col8" class="data row1 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col0" class="data row2 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col1" class="data row2 col1" >2949</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col2" class="data row2 col2" >$1,884,411</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col3" class="data row2 col3" >639</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col4" class="data row2 col4" >76.7</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col5" class="data row2 col5" >81.2</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col6" class="data row2 col6" >77.2%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col7" class="data row2 col7" >94.5%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow2_col8" class="data row2 col8" >85.9%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col0" class="data row3 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col1" class="data row3 col1" >2739</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col2" class="data row3 col2" >$1,763,916</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col3" class="data row3 col3" >644</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col4" class="data row3 col4" >77.1</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col5" class="data row3 col5" >80.7</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col6" class="data row3 col6" >78.2%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col7" class="data row3 col7" >93.9%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow3_col8" class="data row3 col8" >86.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col1" class="data row4 col1" >1468</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col2" class="data row4 col2" >$917,500</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col3" class="data row4 col3" >625</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col4" class="data row4 col4" >83.4</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col5" class="data row4 col5" >83.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col6" class="data row4 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col7" class="data row4 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow4_col8" class="data row4 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col0" class="data row5 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col1" class="data row5 col1" >4635</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col2" class="data row5 col2" >$3,022,020</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col3" class="data row5 col3" >652</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col4" class="data row5 col4" >77.3</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col5" class="data row5 col5" >80.9</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col6" class="data row5 col6" >77.7%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col7" class="data row5 col7" >94.6%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow5_col8" class="data row5 col8" >86.2%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col1" class="data row6 col1" >427</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col2" class="data row6 col2" >$248,087</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col3" class="data row6 col3" >581</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col4" class="data row6 col4" >83.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col5" class="data row6 col5" >83.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col6" class="data row6 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col7" class="data row6 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow6_col8" class="data row6 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col0" class="data row7 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col1" class="data row7 col1" >2917</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col2" class="data row7 col2" >$1,910,635</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col3" class="data row7 col3" >655</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col4" class="data row7 col4" >76.6</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col5" class="data row7 col5" >81.2</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col6" class="data row7 col6" >77.7%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col7" class="data row7 col7" >94.5%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow7_col8" class="data row7 col8" >86.1%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col0" class="data row8 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col1" class="data row8 col1" >4761</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col2" class="data row8 col2" >$3,094,650</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col3" class="data row8 col3" >650</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col4" class="data row8 col4" >77.1</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col5" class="data row8 col5" >81.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col6" class="data row8 col6" >78.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col7" class="data row8 col7" >94.5%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow8_col8" class="data row8 col8" >86.2%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col1" class="data row9 col1" >962</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col2" class="data row9 col2" >$585,858</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col3" class="data row9 col3" >609</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col4" class="data row9 col4" >83.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col5" class="data row9 col5" >84.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col6" class="data row9 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col7" class="data row9 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow9_col8" class="data row9 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col0" class="data row10 col0" >District</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col1" class="data row10 col1" >3999</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col2" class="data row10 col2" >$2,547,363</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col3" class="data row10 col3" >637</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col4" class="data row10 col4" >76.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col5" class="data row10 col5" >80.7</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col6" class="data row10 col6" >77.9%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col7" class="data row10 col7" >94.6%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow10_col8" class="data row10 col8" >86.3%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col0" class="data row11 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col1" class="data row11 col1" >1761</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col2" class="data row11 col2" >$1,056,600</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col3" class="data row11 col3" >600</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col4" class="data row11 col4" >83.4</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col5" class="data row11 col5" >83.7</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col6" class="data row11 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col7" class="data row11 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow11_col8" class="data row11 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col0" class="data row12 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col1" class="data row12 col1" >1635</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col2" class="data row12 col2" >$1,043,130</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col3" class="data row12 col3" >638</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col4" class="data row12 col4" >83.4</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col5" class="data row12 col5" >83.8</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col6" class="data row12 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col7" class="data row12 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow12_col8" class="data row12 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col0" class="data row13 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col1" class="data row13 col1" >2283</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col2" class="data row13 col2" >$1,319,574</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col3" class="data row13 col3" >578</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col4" class="data row13 col4" >83.3</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col5" class="data row13 col5" >84.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col6" class="data row13 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col7" class="data row13 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow13_col8" class="data row13 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_00378c4a_224a_11e8_9cb3_542696dc2a8blevel0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col1" class="data row14 col1" >1800</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col2" class="data row14 col2" >$1,049,400</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col3" class="data row14 col3" >583</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col4" class="data row14 col4" >83.7</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col5" class="data row14 col5" >84.0</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col6" class="data row14 col6" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col7" class="data row14 col7" >100.0%</td> 
        <td id="T_00378c4a_224a_11e8_9cb3_542696dc2a8brow14_col8" class="data row14 col8" >100.0%</td> 
    </tr></tbody> 
</table> 




```python
#Top 5 schools based on Overall Passing % and sorted them from Best to Worst.
top_5_overallPassing = school_summary.sort_values("% Overall Passing", ascending=False)
top_5_overallPassing.head().style.format({"Total Budget": "${:,}", "Average Reading Score": "{:.1f}", 
                                          "Average Math Score": "{:.1f}", "% Passing Math": "{:.1%}", 
                                          "% Passing Reading": "{:.1%}", "% Overall Passing": "{:.1%}"})

```




<style  type="text/css" >
</style>  
<table id="T_01a154c6_224a_11e8_b60a_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_01a154c6_224a_11e8_b60a_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Cabrera High School</th> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col1" class="data row0 col1" >1858</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col2" class="data row0 col2" >$1,081,356</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col3" class="data row0 col3" >582</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col4" class="data row0 col4" >83.1</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col5" class="data row0 col5" >84.0</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col6" class="data row0 col6" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col7" class="data row0 col7" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow0_col8" class="data row0 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_01a154c6_224a_11e8_b60a_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Griffin High School</th> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col1" class="data row1 col1" >1468</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col2" class="data row1 col2" >$917,500</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col3" class="data row1 col3" >625</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col4" class="data row1 col4" >83.4</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col5" class="data row1 col5" >83.8</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col6" class="data row1 col6" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col7" class="data row1 col7" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow1_col8" class="data row1 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_01a154c6_224a_11e8_b60a_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Holden High School</th> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col1" class="data row2 col1" >427</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col2" class="data row2 col2" >$248,087</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col3" class="data row2 col3" >581</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col4" class="data row2 col4" >83.8</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col5" class="data row2 col5" >83.8</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col6" class="data row2 col6" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col7" class="data row2 col7" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow2_col8" class="data row2 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_01a154c6_224a_11e8_b60a_542696dc2a8blevel0_row3" class="row_heading level0 row3" >Pena High School</th> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col1" class="data row3 col1" >962</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col2" class="data row3 col2" >$585,858</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col3" class="data row3 col3" >609</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col4" class="data row3 col4" >83.8</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col5" class="data row3 col5" >84.0</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col6" class="data row3 col6" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col7" class="data row3 col7" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow3_col8" class="data row3 col8" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_01a154c6_224a_11e8_b60a_542696dc2a8blevel0_row4" class="row_heading level0 row4" >Shelton High School</th> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col1" class="data row4 col1" >1761</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col2" class="data row4 col2" >$1,056,600</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col3" class="data row4 col3" >600</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col4" class="data row4 col4" >83.4</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col5" class="data row4 col5" >83.7</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col6" class="data row4 col6" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col7" class="data row4 col7" >100.0%</td> 
        <td id="T_01a154c6_224a_11e8_b60a_542696dc2a8brow4_col8" class="data row4 col8" >100.0%</td> 
    </tr></tbody> 
</table> 




```python
#Bottom 5 schools based on Overall Passing % and sorted them from Worst to Best.
bottom_5_overallPassing = top_5_overallPassing.tail()
bottom_5_overallPassing = school_summary.sort_values("% Overall Passing")
bottom_5_overallPassing.head().style.format({"Total Budget": "${:,}", "Average Reading Score": "{:.1f}", 
                                          "Average Math Score": "{:.1f}", "% Passing Math": "{:.1%}", 
                                          "% Passing Reading": "{:.1%}", "% Overall Passing": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_037d11a4_224a_11e8_935c_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_037d11a4_224a_11e8_935c_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Figueroa High School</th> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col0" class="data row0 col0" >District</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col1" class="data row0 col1" >2949</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col2" class="data row0 col2" >$1,884,411</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col3" class="data row0 col3" >639</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col4" class="data row0 col4" >76.7</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col5" class="data row0 col5" >81.2</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col6" class="data row0 col6" >77.2%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col7" class="data row0 col7" >94.5%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow0_col8" class="data row0 col8" >85.9%</td> 
    </tr>    <tr> 
        <th id="T_037d11a4_224a_11e8_935c_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Ford High School</th> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col0" class="data row1 col0" >District</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col1" class="data row1 col1" >2739</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col2" class="data row1 col2" >$1,763,916</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col3" class="data row1 col3" >644</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col4" class="data row1 col4" >77.1</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col5" class="data row1 col5" >80.7</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col6" class="data row1 col6" >78.2%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col7" class="data row1 col7" >93.9%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow1_col8" class="data row1 col8" >86.0%</td> 
    </tr>    <tr> 
        <th id="T_037d11a4_224a_11e8_935c_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Huang High School</th> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col0" class="data row2 col0" >District</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col1" class="data row2 col1" >2917</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col2" class="data row2 col2" >$1,910,635</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col3" class="data row2 col3" >655</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col4" class="data row2 col4" >76.6</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col5" class="data row2 col5" >81.2</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col6" class="data row2 col6" >77.7%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col7" class="data row2 col7" >94.5%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow2_col8" class="data row2 col8" >86.1%</td> 
    </tr>    <tr> 
        <th id="T_037d11a4_224a_11e8_935c_542696dc2a8blevel0_row3" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col0" class="data row3 col0" >District</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col1" class="data row3 col1" >4635</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col2" class="data row3 col2" >$3,022,020</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col3" class="data row3 col3" >652</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col4" class="data row3 col4" >77.3</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col5" class="data row3 col5" >80.9</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col6" class="data row3 col6" >77.7%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col7" class="data row3 col7" >94.6%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow3_col8" class="data row3 col8" >86.2%</td> 
    </tr>    <tr> 
        <th id="T_037d11a4_224a_11e8_935c_542696dc2a8blevel0_row4" class="row_heading level0 row4" >Johnson High School</th> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col0" class="data row4 col0" >District</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col1" class="data row4 col1" >4761</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col2" class="data row4 col2" >$3,094,650</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col3" class="data row4 col3" >650</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col4" class="data row4 col4" >77.1</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col5" class="data row4 col5" >81.0</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col6" class="data row4 col6" >78.0%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col7" class="data row4 col7" >94.5%</td> 
        <td id="T_037d11a4_224a_11e8_935c_542696dc2a8brow4_col8" class="data row4 col8" >86.2%</td> 
    </tr></tbody> 
</table> 




```python
#Take 9th to 12th grade students per school and take their MATH average. 
gradeNinth_math = students_df.loc[students_df['grade'] == '9th'].groupby('school')["math_score"].mean()
gradeTenth_math = students_df.loc[students_df['grade'] == '10th'].groupby('school')["math_score"].mean()
gradeEleventh_math = students_df.loc[students_df['grade'] == '11th'].groupby('school')["math_score"].mean()
gradeTwelveth_math = students_df.loc[students_df['grade'] == '12th'].groupby('school')["math_score"].mean()

#Create a dataframe with math scores averages of different schools from above
gradeNinthToTweleth_df = pd.DataFrame({"9th": gradeNinth_math,"10th": gradeTenth_math,
                                       "11th": gradeEleventh_math,"12th": gradeTwelveth_math})

#Rearrange them in particular order
gradeNinthToTweleth_df = gradeNinthToTweleth_df[["9th","10th","11th","12th"]]

#Format the data
gradeNinthToTweleth_df.style.format({"9th": "{:.1f}", "10th": "{:.1f}", "11th": "{:.1f}", "12th": "{:.1f}"})

```




<style  type="text/css" >
</style>  
<table id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >school</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow0_col0" class="data row0 col0" >77.1</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow0_col1" class="data row0 col1" >77.0</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow0_col2" class="data row0 col2" >77.5</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow0_col3" class="data row0 col3" >76.5</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow1_col0" class="data row1 col0" >83.1</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow1_col1" class="data row1 col1" >83.2</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow1_col2" class="data row1 col2" >82.8</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow1_col3" class="data row1 col3" >83.3</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow2_col0" class="data row2 col0" >76.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow2_col1" class="data row2 col1" >76.5</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow2_col2" class="data row2 col2" >76.9</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow2_col3" class="data row2 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow3_col0" class="data row3 col0" >77.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow3_col1" class="data row3 col1" >77.7</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow3_col2" class="data row3 col2" >76.9</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow3_col3" class="data row3 col3" >76.2</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow4_col0" class="data row4 col0" >82.0</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow4_col1" class="data row4 col1" >84.2</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow4_col2" class="data row4 col2" >83.8</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow4_col3" class="data row4 col3" >83.4</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow5_col0" class="data row5 col0" >77.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow5_col1" class="data row5 col1" >77.3</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow5_col2" class="data row5 col2" >77.1</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow5_col3" class="data row5 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow6_col0" class="data row6 col0" >83.8</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow6_col1" class="data row6 col1" >83.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow6_col2" class="data row6 col2" >85.0</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow6_col3" class="data row6 col3" >82.9</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow7_col0" class="data row7 col0" >77.0</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow7_col1" class="data row7 col1" >75.9</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow7_col2" class="data row7 col2" >76.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow7_col3" class="data row7 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow8_col0" class="data row8 col0" >77.2</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow8_col1" class="data row8 col1" >76.7</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow8_col2" class="data row8 col2" >77.5</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow8_col3" class="data row8 col3" >76.9</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow9_col0" class="data row9 col0" >83.6</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow9_col1" class="data row9 col1" >83.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow9_col3" class="data row9 col3" >84.1</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow10_col0" class="data row10 col0" >76.9</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow10_col1" class="data row10 col1" >76.6</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow10_col2" class="data row10 col2" >76.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow10_col3" class="data row10 col3" >77.7</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow11_col0" class="data row11 col0" >83.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow11_col1" class="data row11 col1" >82.9</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow11_col2" class="data row11 col2" >83.4</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow11_col3" class="data row11 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow12_col0" class="data row12 col0" >83.6</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow12_col1" class="data row12 col1" >83.1</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow12_col2" class="data row12 col2" >83.5</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow12_col3" class="data row12 col3" >83.5</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow13_col0" class="data row13 col0" >83.1</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow13_col1" class="data row13 col1" >83.7</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow13_col2" class="data row13 col2" >83.2</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow13_col3" class="data row13 col3" >83.0</td> 
    </tr>    <tr> 
        <th id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8blevel0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow14_col0" class="data row14 col0" >83.3</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow14_col1" class="data row14 col1" >84.0</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow14_col2" class="data row14 col2" >83.8</td> 
        <td id="T_c3f36c3a_21af_11e8_8beb_542696dc2a8brow14_col3" class="data row14 col3" >83.6</td> 
    </tr></tbody> 
</table> 




```python
#Take 9th to 12th grade students per school and take their Reading average. 
gradeNinth_reading = students_df.loc[students_df['grade'] == '9th'].groupby('school')["reading_score"].mean()
gradeTenth_reading = students_df.loc[students_df['grade'] == '10th'].groupby('school')["reading_score"].mean()
gradeEleventh_reading = students_df.loc[students_df['grade'] == '11th'].groupby('school')["reading_score"].mean()
gradeTwelveth_reading = students_df.loc[students_df['grade'] == '12th'].groupby('school')["reading_score"].mean()

#Create a dataframe with math scores averages of different schools from above
gradeNinthToTweleth_df = pd.DataFrame({"9th": gradeNinth_reading,"10th": gradeTenth_reading,
                                       "11th": gradeEleventh_reading,"12th": gradeTwelveth_reading})

#Rearrange them in particular order
gradeNinthToTweleth_df = gradeNinthToTweleth_df[["9th","10th","11th","12th"]]

#Format the data
gradeNinthToTweleth_df.style.format({"9th": "{:.1f}", "10th": "{:.1f}", "11th": "{:.1f}", "12th": "{:.1f}"})
```




<style  type="text/css" >
</style>  
<table id="T_eee60b46_21af_11e8_a519_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >school</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow0_col0" class="data row0 col0" >81.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow0_col1" class="data row0 col1" >80.9</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow0_col2" class="data row0 col2" >80.9</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow0_col3" class="data row0 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow1_col0" class="data row1 col0" >83.7</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow1_col1" class="data row1 col1" >84.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow1_col2" class="data row1 col2" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow1_col3" class="data row1 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow2_col0" class="data row2 col0" >81.2</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow2_col2" class="data row2 col2" >80.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow2_col3" class="data row2 col3" >81.4</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow3_col0" class="data row3 col0" >80.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow3_col1" class="data row3 col1" >81.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow3_col2" class="data row3 col2" >80.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow3_col3" class="data row3 col3" >80.7</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow4_col0" class="data row4 col0" >83.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow4_col1" class="data row4 col1" >83.7</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow4_col2" class="data row4 col2" >84.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow4_col3" class="data row4 col3" >84.0</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow5_col0" class="data row5 col0" >80.9</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow5_col1" class="data row5 col1" >80.7</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow5_col2" class="data row5 col2" >81.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow5_col3" class="data row5 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow6_col0" class="data row6 col0" >83.7</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow6_col1" class="data row6 col1" >83.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow6_col2" class="data row6 col2" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow6_col3" class="data row6 col3" >84.7</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow7_col0" class="data row7 col0" >81.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow7_col1" class="data row7 col1" >81.5</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow7_col2" class="data row7 col2" >81.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow7_col3" class="data row7 col3" >80.3</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow8_col0" class="data row8 col0" >81.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow8_col1" class="data row8 col1" >80.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow8_col2" class="data row8 col2" >80.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow8_col3" class="data row8 col3" >81.2</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow9_col0" class="data row9 col0" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow9_col1" class="data row9 col1" >83.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow9_col3" class="data row9 col3" >84.6</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow10_col0" class="data row10 col0" >81.0</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow10_col1" class="data row10 col1" >80.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow10_col2" class="data row10 col2" >80.9</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow10_col3" class="data row10 col3" >80.4</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow11_col0" class="data row11 col0" >84.1</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow11_col1" class="data row11 col1" >83.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow11_col2" class="data row11 col2" >84.4</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow11_col3" class="data row11 col3" >82.8</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow12_col0" class="data row12 col0" >83.7</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow12_col1" class="data row12 col1" >84.3</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow12_col2" class="data row12 col2" >83.6</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow12_col3" class="data row12 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow13_col0" class="data row13 col0" >83.9</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow13_col1" class="data row13 col1" >84.0</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow13_col2" class="data row13 col2" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow13_col3" class="data row13 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_eee60b46_21af_11e8_a519_542696dc2a8blevel0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow14_col0" class="data row14 col0" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow14_col1" class="data row14 col1" >83.8</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow14_col2" class="data row14 col2" >84.2</td> 
        <td id="T_eee60b46_21af_11e8_a519_542696dc2a8brow14_col3" class="data row14 col3" >84.1</td> 
    </tr></tbody> 
</table> 




```python
#Create bins based on spending per student

bins = [0, 584.999, 614.999, 644.999, 999999]
bins_names = ["< $585", "$585 - $614", "$615 - $644", "> $644"]
merged_df['spending_bins'] = pd.cut(merged_df['budget']/merged_df['size'], bins, labels = bins_names)

groupby_spending = merged_df.groupby("spending_bins")

avg_math1 = groupby_spending["math_score"].mean()
avg_reading1 = groupby_spending["reading_score"].mean()
passing_math1 = merged_df[merged_df["math_score"] >= 65].groupby("spending_bins")["Student ID"].count()/groupby_spending["Student ID"].count()
passing_reading1 = merged_df[merged_df["reading_score"] >= 65].groupby("spending_bins")["Student ID"].count()/groupby_spending["Student ID"].count()
overall_passing1 = (passing_math1+passing_reading1)/2

#Create dataframe
scr_by_schlSpeding = pd.DataFrame({"Average Math Score":avg_math1,"Average Reading score":avg_reading1,
                                   "% Passing Math":passing_math1,"% Passing Reading":passing_reading1,
                                   "Overall Passing Rate":overall_passing1})
#Organize the column order
scr_by_schlSpeding = scr_by_schlSpeding[["Average Math Score","Average Reading score", "% Passing Math", 
                                         "% Passing Reading","Overall Passing Rate"]]
scr_by_schlSpeding.index.name = "Spending Ranges (Per Student)"
#Format
scr_by_schlSpeding.style.format({"Average Math Score": "{:.1f}","Average Reading Score": "{:.1f}",
                                 "% Passing Math": "{:.1%}","% Passing Reading":"{:.1%}","Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_1b14c6ba_224d_11e8_be08_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Spending Ranges (Per Student)</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1b14c6ba_224d_11e8_be08_542696dc2a8blevel0_row0" class="row_heading level0 row0" >< $585</th> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow0_col1" class="data row0 col1" >83.964</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow0_col2" class="data row0 col2" >100.0%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow0_col3" class="data row0 col3" >100.0%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow0_col4" class="data row0 col4" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_1b14c6ba_224d_11e8_be08_542696dc2a8blevel0_row1" class="row_heading level0 row1" >$585 - $614</th> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow1_col0" class="data row1 col0" >83.5</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow1_col1" class="data row1 col1" >83.8384</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow1_col2" class="data row1 col2" >100.0%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow1_col3" class="data row1 col3" >100.0%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow1_col4" class="data row1 col4" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_1b14c6ba_224d_11e8_be08_542696dc2a8blevel0_row2" class="row_heading level0 row2" >$615 - $644</th> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow2_col0" class="data row2 col0" >78.1</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow2_col1" class="data row2 col1" >81.4341</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow2_col2" class="data row2 col2" >81.7%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow2_col3" class="data row2 col3" >95.4%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow2_col4" class="data row2 col4" >88.6%</td> 
    </tr>    <tr> 
        <th id="T_1b14c6ba_224d_11e8_be08_542696dc2a8blevel0_row3" class="row_heading level0 row3" >> $644</th> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow3_col0" class="data row3 col0" >77.0</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow3_col1" class="data row3 col1" >81.0056</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow3_col2" class="data row3 col2" >77.8%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow3_col3" class="data row3 col3" >94.5%</td> 
        <td id="T_1b14c6ba_224d_11e8_be08_542696dc2a8brow3_col4" class="data row3 col4" >86.2%</td> 
    </tr></tbody> 
</table> 




```python
#Create bins based on spending per student

bins = [0, 999, 1999, 10000]
bins_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
merged_df['size_bins'] = pd.cut(merged_df['size'], bins, labels = bins_names)

groupby_size = merged_df.groupby("size_bins")

avg_math1 = groupby_size["math_score"].mean()
avg_reading1 = groupby_size["reading_score"].mean()
passing_math1 = merged_df[merged_df["math_score"] >= 65].groupby("size_bins")["Student ID"].count()/groupby_size["Student ID"].count()
passing_reading1 = merged_df[merged_df["reading_score"] >= 65].groupby("size_bins")["Student ID"].count()/groupby_size["Student ID"].count()
overall_passing1 = (passing_math1+passing_reading1)/2

#Create dataframe
scores_by_size = pd.DataFrame({"Average Math Score":avg_math1,"Average Reading score":avg_reading1,
                                   "% Passing Math":passing_math1,"% Passing Reading":passing_reading1,
                                   "Overall Passing Rate":overall_passing1})
#Organize the column order
scores_by_size = scores_by_size[["Average Math Score","Average Reading score", "% Passing Math", 
                                         "% Passing Reading","Overall Passing Rate"]]
scores_by_size.index.name = "School Size"
#Format
scores_by_size.style.format({"Average Math Score": "{:.1f}","Average Reading Score": "{:.1f}",
                                 "% Passing Math": "{:.1%}","% Passing Reading":"{:.1%}","Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_95bcb96c_224e_11e8_aa65_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School Size</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_95bcb96c_224e_11e8_aa65_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Small (<1000)</th> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow0_col0" class="data row0 col0" >83.8</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow0_col1" class="data row0 col1" >83.9741</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow0_col2" class="data row0 col2" >100.0%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow0_col3" class="data row0 col3" >100.0%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow0_col4" class="data row0 col4" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_95bcb96c_224e_11e8_aa65_542696dc2a8blevel0_row1" class="row_heading level0 row1" >Medium (1000-2000)</th> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow1_col0" class="data row1 col0" >83.4</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow1_col1" class="data row1 col1" >83.868</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow1_col2" class="data row1 col2" >100.0%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow1_col3" class="data row1 col3" >100.0%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow1_col4" class="data row1 col4" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_95bcb96c_224e_11e8_aa65_542696dc2a8blevel0_row2" class="row_heading level0 row2" >Large (2000-5000)</th> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow2_col0" class="data row2 col0" >77.5</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow2_col1" class="data row2 col1" >81.1987</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow2_col2" class="data row2 col2" >79.6%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow2_col3" class="data row2 col3" >94.9%</td> 
        <td id="T_95bcb96c_224e_11e8_aa65_542696dc2a8brow2_col4" class="data row2 col4" >87.2%</td> 
    </tr></tbody> 
</table> 




```python
#Scores by Type of school

groupby_type = merged_df.groupby("type")

avg_math1 = groupby_type["math_score"].mean()
avg_reading1 = groupby_type["reading_score"].mean()
passing_math1 = merged_df[merged_df["math_score"] >= 65].groupby("type")["Student ID"].count()/groupby_type["Student ID"].count()
passing_reading1 = merged_df[merged_df["reading_score"] >= 65].groupby("type")["Student ID"].count()/groupby_type["Student ID"].count()
overall_passing1 = (passing_math1+passing_reading1)/2

#Create dataframe
by_type = pd.DataFrame({"Average Math Score":avg_math1,"Average Reading score":avg_reading1,
                                   "% Passing Math":passing_math1,"% Passing Reading":passing_reading1,
                                   "Overall Passing Rate":overall_passing1})
#Organize the column order
by_type = by_type[["Average Math Score","Average Reading score", "% Passing Math", 
                                         "% Passing Reading","Overall Passing Rate"]]
by_type.index.name = "School Size"
#Format
by_type.style.format({"Average Math Score": "{:.1f}","Average Reading Score": "{:.1f}",
                                 "% Passing Math": "{:.1%}","% Passing Reading":"{:.1%}","Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School Size</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8blevel0_row0" class="row_heading level0 row0" >Charter</th> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow0_col1" class="data row0 col1" >83.9028</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow0_col2" class="data row0 col2" >100.0%</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow0_col3" class="data row0 col3" >100.0%</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow0_col4" class="data row0 col4" >100.0%</td> 
    </tr>    <tr> 
        <th id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8blevel0_row1" class="row_heading level0 row1" >District</th> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow1_col0" class="data row1 col0" >77.0</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow1_col1" class="data row1 col1" >80.9625</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow1_col2" class="data row1 col2" >77.8%</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow1_col3" class="data row1 col3" >94.5%</td> 
        <td id="T_9c1b39e6_224e_11e8_8e65_542696dc2a8brow1_col4" class="data row1 col4" >86.2%</td> 
    </tr></tbody> 
</table> 


