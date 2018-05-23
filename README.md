
# Data Visualization with D3 and Dimple.js

In this project, we'll take a look at our Prosper Loan data again from the Exploratory Data Analysis project.  Since our dataset is quite large, I've chosen to take a closer look at the Wisconsin loans in particular for this project.  I chose Wisconsin because it is my home state and I was curious to see what credit scores and interest rates are common for personal loans from Prosper in this area.

[Jump to Project Requirements](#Requirements)


```python
#import Prosper Loan Data and write some of the more important variables to csv for analysis

import csv

with open('prosperLoanData.csv', 'r') as f:
    loanData = csv.reader(f)
    with open('LoanChartData.csv','w') as chart:
        writer = csv.writer(chart)
        for row in loanData:
            writer.writerow(row[2:3]+row[4:9]+row[17:20]+row[26:27])
        
```


```python
with open('LoanChartData.csv','r') as f:
    chartData = csv.reader(f)
    count = 0
    for row in chartData:
        if count < 2:
            print row
        count += 1
    
```

    ['ListingCreationDate', 'Term', 'LoanStatus', 'ClosedDate', 'BorrowerAPR', 'BorrowerRate', 'BorrowerState', 'Occupation', 'EmploymentStatus', 'CreditScoreRangeUpper']
    ['2007-08-26 19:09:29.263000000', '36', 'Completed', '2009-08-14 00:00:00', '.165160', '.158000', 'CO', 'Other', 'Self-employed', '659']
    


```python
import pandas as pd

dfLoanData = pd.read_csv('LoanChartData.csv')
```


```python
#change credit score to category

def credit_score(score):
    
    category = None
    
    if score < 550.0:
        category = 'Bad'
    if score >= 550.0 and score < 650.0:
        category = 'Poor'
    if score >= 650.0 and score < 700.0:
        category = 'Fair'
    if score >= 700.0 and score < 750.0:
        category = 'Good'
    if score >= 750.0:
        category = 'Excellent'
        
    return category
```


```python
dfLoanData['Credit'] = map(credit_score,dfLoanData['CreditScoreRangeUpper'])
```


```python
dfLoanData['Credit'].head()
```




    0         Fair
    1         Fair
    2          Bad
    3    Excellent
    4         Fair
    Name: Credit, dtype: object




```python
#Borrower rate Range

def rate_range(rate):
    r_range = None
    
    if rate < 0.05:
        r_range = "0-4.99%"
    if rate >= 0.05 and rate < 0.10:
        r_range = "5-9.99%"
    if rate >= 0.10 and rate < 0.15:
        r_range = "10-14.99%"
    if rate >= 0.15 and rate < 0.20:
        r_range = "15-19.99%"
    if rate >= 0.20 and rate < 0.25:
        r_range = "20-24.99%"
    if rate >= 0.25 and rate < 0.30:
        r_range = "25-29.99%"
    if rate >= 0.30 and rate < 0.35:
        r_range = "30-34.99%"
    if rate >= 0.35 and rate < 0.40:
        r_range = "35-39.99%"
    if rate >= 0.40 and rate < 0.45:
        r_range = "40-44.99%"
    if rate >= 0.45 and rate < 0.50:
        r_range = "45-49.99%"
    
    return r_range
    
        
```


```python
dfLoanData['Rate_Range'] = map(rate_range,dfLoanData['BorrowerRate'])
```


```python
#add 'Count' column for y-axis

for row in dfLoanData:
    dfLoanData['Count'] = 1
```


```python
dfLoanData.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 113937 entries, 0 to 113936
    Data columns (total 13 columns):
    ListingCreationDate      113937 non-null object
    Term                     113937 non-null int64
    LoanStatus               113937 non-null object
    ClosedDate               55089 non-null object
    BorrowerAPR              113912 non-null float64
    BorrowerRate             113937 non-null float64
    BorrowerState            108422 non-null object
    Occupation               110349 non-null object
    EmploymentStatus         111682 non-null object
    CreditScoreRangeUpper    113346 non-null float64
    Credit                   113346 non-null object
    Rate_Range               113937 non-null object
    Count                    113937 non-null int64
    dtypes: float64(3), int64(2), object(8)
    memory usage: 11.3+ MB
    


```python
dfLoanData['Count'].head()
```




    0    1
    1    1
    2    1
    3    1
    4    1
    Name: Count, dtype: int64




```python
WI_Prosper_Loan_Data = dfLoanData[dfLoanData.BorrowerState.isin(['WI'])]
WI_Prosper_Loan_Data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ListingCreationDate</th>
      <th>Term</th>
      <th>LoanStatus</th>
      <th>ClosedDate</th>
      <th>BorrowerAPR</th>
      <th>BorrowerRate</th>
      <th>BorrowerState</th>
      <th>Occupation</th>
      <th>EmploymentStatus</th>
      <th>CreditScoreRangeUpper</th>
      <th>Credit</th>
      <th>Rate_Range</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>2012-09-21 13:37:43.210000000</td>
      <td>36</td>
      <td>Current</td>
      <td>NaN</td>
      <td>0.35797</td>
      <td>0.3177</td>
      <td>WI</td>
      <td>Other</td>
      <td>Other</td>
      <td>699.0</td>
      <td>Fair</td>
      <td>30-34.99%</td>
      <td>1</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2012-08-26 09:43:38.643000000</td>
      <td>60</td>
      <td>Current</td>
      <td>NaN</td>
      <td>0.20931</td>
      <td>0.1852</td>
      <td>WI</td>
      <td>Executive</td>
      <td>Employed</td>
      <td>739.0</td>
      <td>Good</td>
      <td>15-19.99%</td>
      <td>1</td>
    </tr>
    <tr>
      <th>105</th>
      <td>2011-08-16 20:24:41.713000000</td>
      <td>36</td>
      <td>Chargedoff</td>
      <td>2012-04-01 00:00:00</td>
      <td>0.30532</td>
      <td>0.2699</td>
      <td>WI</td>
      <td>Sales - Commission</td>
      <td>Employed</td>
      <td>659.0</td>
      <td>Fair</td>
      <td>25-29.99%</td>
      <td>1</td>
    </tr>
    <tr>
      <th>137</th>
      <td>2013-12-30 09:12:58.487000000</td>
      <td>36</td>
      <td>Current</td>
      <td>NaN</td>
      <td>0.21648</td>
      <td>0.1795</td>
      <td>WI</td>
      <td>Construction</td>
      <td>Employed</td>
      <td>679.0</td>
      <td>Fair</td>
      <td>15-19.99%</td>
      <td>1</td>
    </tr>
    <tr>
      <th>182</th>
      <td>2012-04-20 13:37:22.147000000</td>
      <td>36</td>
      <td>Current</td>
      <td>NaN</td>
      <td>0.25259</td>
      <td>0.2148</td>
      <td>WI</td>
      <td>Computer Programmer</td>
      <td>Employed</td>
      <td>699.0</td>
      <td>Fair</td>
      <td>20-24.99%</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
WI_Prosper_Loan_Data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1842 entries, 31 to 113868
    Data columns (total 13 columns):
    ListingCreationDate      1842 non-null object
    Term                     1842 non-null int64
    LoanStatus               1842 non-null object
    ClosedDate               793 non-null object
    BorrowerAPR              1842 non-null float64
    BorrowerRate             1842 non-null float64
    BorrowerState            1842 non-null object
    Occupation               1810 non-null object
    EmploymentStatus         1837 non-null object
    CreditScoreRangeUpper    1842 non-null float64
    Credit                   1842 non-null object
    Rate_Range               1842 non-null object
    Count                    1842 non-null int64
    dtypes: float64(3), int64(2), object(8)
    memory usage: 201.5+ KB
    


```python
print "WI Min Rate", WI_Prosper_Loan_Data.BorrowerRate.min()
print "WI Max Rate", WI_Prosper_Loan_Data.BorrowerRate.max()
```

    WI Min Rate 0.055
    WI Max Rate 0.36
    


```python
print "WI Min Upper Credit Score", WI_Prosper_Loan_Data.CreditScoreRangeUpper.min()
print "WI Max Upper Credit Score", WI_Prosper_Loan_Data.CreditScoreRangeUpper.max()
```

    WI Min Upper Credit Score 479.0
    WI Max Upper Credit Score 859.0
    


```python
#WI Loan Data csv for visualization

WI_Prosper_Loan_Data.to_csv('WI_Loan_Data.csv')
```

<a id='Requirements'></a>

Requirements:

- Summary - in no more than 4 sentences, briefly introduce your data visualization and add any context that can help readers understand it
- Design - explain any design choices you made including changes to the visualization after collecting feedback
- Feedback - include all feedback you received from others on your visualization from the first sketch to the final visualization
- Resources - list any sources you consulted to create your visualization

## Summary

My data visualization built using d3, dimple.js, was created using a portion of the [Prosper Loan Dataset]('https://www.kaggle.com/jschnessl/prosperloans/data').  I extracted all of the loans with 'BorrowerState' equal to 'WI', as Wisconsin is my home state.  My goal was to show the change in the number of people with each type of credit as interest rates rise, thus revealing that Wisconsinites with Good/Excellent credit are more likely to get lower rates.  Showing each group by term adds more detail to the narrative, as 36 month terms are more common and rates may differ depending on the term.  

## Design

I originally thought I'd use a bubble plot, showing various sized circles for each of the three loan terms.  The chart looked a bit cluttered to me and didn't look like the best way to visualize drastic changes in number of people per group.  The bar chart looked much better and the animation gave me just what I was looking for.  It was easy to interpret and had a clear 'story' to tell.  When designing the bar chart, I started out by including the 'Term' variable within the x-axis.  But, that took away from the story I was trying to tell and kept the viewers eyes on the difference between loan terms instead of what should be an obvious trend between credit type and interest rates.  After getting further feedback, I changed my barplot to a dodged bar plot in order to give the viewer a way to compair each rate group side by side.  I also added interaction capabilities to engage the viewer as they reveal that only borrowers with Good/Excellent credit get the best/lowest rates, and those with Bad credit are going to get the higher rates.

## Feedback and Improvements

See Revisions at https://gist.github.com/missmariss31/0406d32621fba51797489339da3bfeb5

<b>First Draft:</b>
- After getting some feedback on an earlier visualization, I changed the storyboard interval to allow the viewer more time to take in each transition through interest rate ranges.  I also added the 'Count' to the y-axis after I realized it was summing up all of the values in my earlier selection of 'BorrowerRate' and not counting them.  I thought about using the bubble chart, but I felt that it caused crowding and didn't tell the story in the best way.

<b>Second Draft:</b>
- I took out the sectioning of the x axis that included 'Term' as it didn't add anything of value.  In fact, it took away from the credit/rate correlation story.  After further feedback, I made the legend stop blinking with every transition (I guess that's "annoying") by adding 'myChart.legends = [];' after the chart has been drawn. 

<b>Third Draft:</b>
- After getting my third review of the visualization, I cleaned it up as much as I could without distracting from the goal/story.  I increased font size and gave the x-axis a clear label, 'Credit Type.'  I also changed the title to reflect the variables used as well as the dataset.

<b>Fourth Draft:</b>
- After some great feedback from my project reviewer, I got back to work and changed the single bar plot to a dodged bar plot to make it easier for the viewer to compare the difference in rate groups.  I also added interaction and changed the animation to reflect the difference in loans with 36month, 60month, and 12month terms.  In order to give the viewer an idea of what they might discover, prompting them to isolate the lower rates, I added a small header.  I was also able to give the viewer a way to start/stop the animation in order for them to take in all of the detail in each frame.

In order to view the final data visualization, go to http://bl.ocks.org/missmariss31/raw/0406d32621fba51797489339da3bfeb5/

## Resources

Documentation
- https://github.com/PMSI-AlignAlytics/dimple/wiki

Examples
- http://dimplejs.org/advanced_examples_index.html


```python

```
