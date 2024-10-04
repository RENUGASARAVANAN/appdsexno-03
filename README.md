# AppDSExno-03

**AIM:**

To Implement Recommendation Systems using the suitable data sets.

**ALGORITHM:**

STEP 1: Load the necessary Datasets.

STEP 2: Include the necessary python library.

STEP 3: Use Fuzzy library for handling text data.

STEP 4: Perform Data Preprocessing Steps.

STEP 5: Standardize column names for merging.

STEP 6: Apply fuzzy matching to find similar text data between datasets.

STEP 7: Perform Data transformation between datasets.

STEP 8: Define a recommendation score using the features of the datasets.

STEP 9: Sort the data by recommendation score.

STEP 10: Export the results to a CSV file.


**PROGRAM:**
```
DEVELOPED BY: RENUGA S
REG NO : 212222230118
```
```
import pandas as pd 
import numpy as np
from fuzzywuzzy import process
from sklearn.preprocessing import MinMaxScaler
```
<table>
<tr>
<td>
    
```
da=pd.read_csv('emobile.csv')          
da.head()
```
</td>

<td>

![Screenshot 2024-10-04 085804](https://github.com/user-attachments/assets/c8ee96ae-7511-4b8b-a116-89f1e29f1e3e)
</td>
</tr>
</table>

<table>
<tr>
<td>
  

```
db=pd.read_csv('maxmobile.csv')          
db.head()
```

</td>

<td>

![Screenshot 2024-10-04 085812](https://github.com/user-attachments/assets/9208a077-97b1-4960-bcf1-a546a6d8346d)


</td>
</tr>
</table>

```
da.drop_duplicates(inplace=True)
db.drop_duplicates(inplace=True)
da.columns=['Product_ID','Product_Name','Rating','Review_Count']
db.columns=['Product_ID','Product_Name','Rating','Review_Count']
```

```
def matching(name,choice,l=1):
    results=process.extract(name,choice,limit=l)
    return results[0][0] if results else None
```

```
db['Matched_Product'] = db['Product_Name'].apply(lambda x: matching(x, da['Product_Name'].tolist()))
db.head()
```
![Screenshot 2024-10-04 085825](https://github.com/user-attachments/assets/cdd2aa50-9bce-44c0-957c-8cb5f4dcc626)


```
merged=pd.merge(da,db,left_on='Product_Name',right_on='Matched_Product',
                how='inner',suffixes=('_ds1','_ds2'))
merged.head()

```
![Screenshot 2024-10-04 090011](https://github.com/user-attachments/assets/76e131ad-85fd-4f37-90b5-e2f9df717252)



```
merged.columns
x=merged['Review_Count_ds1']+merged['Review_Count_ds2']
merged['Combined_Rating']=(merged['Rating_ds1']*merged['Review_Count_ds1']+
                           merged['Rating_ds2']*merged['Review_Count_ds2'])/x
merged.head()
```
![Screenshot 2024-10-04 090022](https://github.com/user-attachments/assets/ea22b38b-959c-4360-bb86-4cc9dc6b7131)




```
merged['Recommendation_Score']=0.7*merged['Combined_Rating']+0.3*merged['Rating_ds1']
merged.head()
```

![Screenshot 2024-10-04 090034](https://github.com/user-attachments/assets/4e475028-11db-4e9d-a7dd-e2a75f0c5636)


```
best=merged.sort_values (by='Recommendation_Score', ascending=False)
print(best[['Matched_Product', 'Combined_Rating', 'Recommendation_Score']].head(5)
```
![Screenshot 2024-10-04 090042](https://github.com/user-attachments/assets/a9e1f0f2-93c9-4f49-a7a1-d11cda6deebb)



```
best[['Matched_Product','Combined_Rating',
      'Recommendation_Score']].to_csv('BestRecommended.csv',index=False)
new=pd.read_csv('BestRecommended.csv')
new.head()
```
![Screenshot 2024-10-04 090049](https://github.com/user-attachments/assets/cccc3132-d769-48a5-baa6-6446dd6de09f)


**RESULT**

Thus, the Implementation of the Recommendation System for the given dataset is completed successfully.
