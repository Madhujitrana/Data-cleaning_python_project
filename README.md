# Data-cleaning_python_project
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt




df=pd.read_csv("C:/Users/Madhujit/Downloads/ReactionTypes.csv")

## data info

df.info()

## check 10 line of data

df.head(10)

## remove the first column as it is unrellevent

df.drop(["Unnamed: 0"],axis=1,inplace=True)


## Check the null value in dataset and clear it out

df.isnull().sum()

##check the duplicate in the dataset and clear it out

df.duplicated().sum()

## checking of outlier in the dataset

fig,axes=plt.subplots(figsize=(14,5))

for i in df["Score"]:
    sns.boxplot(data=df,y=df["Score"],ax=axes)
    plt.show()

### final statement: data is completely clean


#####cleaning of 2nd dataset Reaction dataset

df=pd.read_csv("C:/Users/Madhujit/Downloads/Reactions.csv")

## data info

df.info()

## A the datetime is in object lets convert it to datetime format
## and drop the Datetime columns

df["Date"]=pd.to_datetime(df["Datetime"])

df.drop(["Datetime"],axis=1,inplace=True)

## check 10 line of data

df.head(10)

## remove the first column as it is unrellevent

df.drop(["Unnamed: 0"],axis=1,inplace=True)


## Check the null value in dataset and clear it out

df.isnull().sum()

### As there has null value so check the percentage of null in dataset
## if it is low then will remove it or we will impute with knn imputer
c={}
for i in df.columns:
    x=df[i].isnull().sum()/df.shape[0]
    c[i]=x
print(c)

## decided to drop the null values from dataset

df.dropna(axis=0,inplace=True)

### Again run to see the null values

c={}
for i in df.columns:
    x=df[i].isnull().sum()/df.shape[0]
    c[i]=x
print(c)

df.head(10)


##check the duplicate in the dataset and clear it out

df.duplicated().sum()

##Replace the column with _
df1=df.copy()
df.columns=df.columns.str.replace(" ","_")

df.isnull().sum()

### Export the data to excel
df.to_csv("Reaction.csv",index=False)



#### Cleanup the 3rd dataset

df=pd.read_csv("C:/Users/Madhujit/Downloads/Content.csv")

## data info

df.info()


## check 10 line of data

df.head(10)

## remove the first column as it is unrellevent

df.drop(["Unnamed: 0"],axis=1,inplace=True)


## Check the null value in dataset and clear it out

df.isnull().sum()

### As there has null value so check the percentage of null in dataset
## if it is low then will remove it or we will impute with knn imputer
c={}
for i in df.columns:
    x=df[i].isnull().sum()/df.shape[0]
    c[i]=x
print(c)

## decided to fill with no_avilable inplace of null value

df.fillna("Not_Avilable",inplace=True)

### Again run to see the null values

c={}
for i in df.columns:
    x=df[i].isnull().sum()/df.shape[0]
    c[i]=x
print(c)

df.head(10)


##check the duplicate in the dataset and clear it out

df.duplicated().sum()

##Replace the column with _
df1=df.copy()
df.columns=df.columns.str.replace(" ","_")

### Export the data to excel
df.to_csv("Content_Clean.csv",index=False)


#### Merging the 3 table togather to create a data final dataset


df1=pd.read_csv("C:/Users/Madhujit/Downloads/ReactionTypes.csv")
df2=pd.read_csv("C:/Users/Madhujit/Downloads/Reaction.csv")
df3=pd.read_csv("C:/Users/Madhujit/Downloads/Content_Clean.csv")


### check the columns of each data

df1.columns
df2.columns
df3.columns

final_data=pd.merge(df1,df2,on="Type",how="left")

final_data=pd.merge(final_data,df3,on="Content_ID",how="left")

final_data.drop(["User_ID_y","Type_y",'Unnamed: 0'],axis=1,inplace=True)

final_data

### find out the yop 5 category

final_data.columns


top_category=final_data.groupby("Category").agg({"Score":sum})

top_five_category=top_category.sort_values(by="Score",ascending=False)

final_top_5_category=top_five_category[:5]

final_data.to_csv("final_data.csv",index=False)

final_top_5_category.to_excel("final_top_5_category.xlsx",index=False)

final_top_5_category=final_top_5_category.reset_index()
