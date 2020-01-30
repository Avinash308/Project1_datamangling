# Project1_datamangling
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
df=pd.read_csv("https://raw.githubusercontent.com/jackiekazil/data-wrangling/master/data/chp3/data-text.csv")
df.head(2)

df1=pd.read_csv("https://raw.githubusercontent.com/kjam/data-wrangling-pycon/master/data/berlin_weather_oldest.csv")
df1.head(2)

#to get information of meta data use the below code
df.info()
df1.info()

#getting row names of both data sets
rows=np.array(df.index.array)
rows

rows1=np.array(df1.index.array)
rows1

#changing column name from the above dataset
df.rename(columns={"Indicator":"Indicator_id"}).head(2)

df.rename(columns={"Indicator":"Indicator_id"},inplace=True)
df.head(2)

#changing names of multiple columns
df.rename(columns={"PUBLISH STATES":"Publish states","WHO region":"WHO Region"},inplace=True)
df.head(2)

#Arrange values of particular column in ascending order
df.sort_values(by=["Year"]).head(2)

#Arranging multiple column values in ascending order
df.sort_values(by=['Country','Year','WHO Region','Publish states'],inplace=True)
df

df=df.reindex(columns=['Country']+[a for a in df.columns if a!='Country'])
df

#getting column array using a variable
df['Country'].values

#here's how to get the subset rows
df_temp=df.loc[[11,24,37],:]
df_temp

#excute all the rows excluding 5,12,23,56
df_temp1=df.drop(df.index[[5,12,23,56]])
df_temp1


users=pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/users.csv')
sessions=pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/sessions.csv')
products=pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/products.csv')
transactions=pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/transactions.csv')
users.head()

sessions.head()
transactions.head()
s1=pd.merge(users,transactions,how='left')
s1


s2=pd.merge(transactions,users,how="left")
s2

#which transactions have a user_id not in users
df3=transactions[s2['UserID'].isin(users['UserID'])==False]
df3

#Join users to transactions, keeping only rows from transactions and users that match via UserID (inner join)
df4=pd.merge(transactions,users,how='inner',on='UserID')
df4

#Join users to transactions, displaying all matching rows AND all non-matching rows (full outer join)
df5=pd.merge(transactions,users,how='outer')
df5

#determine which session occured on the same day each user registered 
df6=pd.merge(users,sessions,how="outer",on='UserID')
df6[df6['Registered']==df6['SessionDate']]
df6

#Build a dataset with every possible (UserID, ProductID) pair (cross join)

df8=df5.iloc[:,2:4]
df8.dropna(inplace=True)
df8

#determine how much quantity of each product was purchased by each user

tolist=[(i,j)for i in df8['UserID']for j in df8['ProductID']]
tolist=list(dict.fromkeys(tolist))
df9=pd.DataFrame(data=tolist,columns=['UserID','ProductID'])
df9.head(3)

df1 = pd.DataFrame({'key': np.repeat(1, users.shape[0]), 'UserID': users.UserID})
df2 = pd.DataFrame({'key': np.repeat(1, products.shape[0]), 'ProductID': products.ProductID})
user_products = pd.merge(df1, df2,on='key')[['UserID', 'ProductID']]
pd.merge(user_products, transactions, how='left', on=['UserID', 'ProductID']).groupby(['UserID', 'ProductID']).apply(lambda x: pd.Series(dict(
    Quantity=x.Quantity.sum()
))).reset_index().fillna(0)

#for each user get each possible pair of pair transactions (TransactionID1,TransactionID2)
pd.merge(transactions,transactions,on='UserID')

#Join each user to his/her first ocuuring transaction in the transaction table
data=pd.merge(users, transactions.groupby('UserID').first().reset_index(), how='left', on='UserID')
data


#Test to see if we can drop columns

columns = list(data.columns)
list(data.dropna(thresh=int(data.shape[0] * .9), axis=1).columns)
missingInfo = list(data.columns[data.isnull().any()])
missingInfo

for col in missingInfo:
    missingNumber = data[data[col].isnull() == True].shape[0]
    print('Missing Number for Col {}: {}'.format(col, missingNumber))
    
for col in missingInfo:
    percentMissing = data[data[col].isnull() == True].shape[0] / data.shape[0]
    print('Col Percent Missing {}: {}'.format(col, percentMissing))










