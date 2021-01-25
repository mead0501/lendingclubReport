# Lending Club Report
## Mead Cheung
*Lending club data analytics*

**For this project, we present and explore the data from_lending club_.**

### 1. About Lending Club

-[Lending club](https://www.lendingclub.com/) is a P2P lending platform.

-It is one of the largest market connecting borrowers and investors.

-Its aim is to transform the banking system to make credit more affordable and investing more rewarding.

-Borrowers use loans to consolidate debt, improve their homes, finance major purchases, and more.

-Investors purchase Notes, which correspond to fractions of loans, to potentially earn competitive returns.


### 2.Data Explore

-2.1Read the file loan-clean-shiyan.csv and observe it, and find that there are a large number of invalid values in the file
###### data1=pd.read_csv(r'F:\****\***\Python\**预测\3.8\loan-clean-shiyan.csv')
###### data1

-2.2To reduce programming interference, turn off warnings

###### import warnings

###### warnings.filterwarnings('ignore')

### 3.View the data and convert member_id from float type to string type

###### data1.info()
###### data1.member_id=data1.member_id.astype('str')

-3.1Handle missing values in rows

###### data2=data1.loc[(~data1.member_id.isin(['nan','Null','NaN','None']))]

-3.2 Handle missing values in columns

###### data3=data2.drop(data2.columns[50:74],axis=1)

-3.3 Handle duplicate values, no duplicate values found

###### data3.shape

###### data3.drop_duplicates().shape

### 4. Modify

-4.1Change the column name you need

###### data3=data3.rename(columns={'last_pymnt_amnt':'lastpay','int_rate':'rate'})

-4.2 Adjust the order

###### data4=data3.loc[:,['member_id','grade','annual_inc','loan_amnt','term','rate','installment','lastpay']]

### 5.Save the output

###### data4.to_csv('data_BA1.csv',index=False)

### 6.Read the newly saved file and plot

###### Ls

###### df=pd.read_csv('data_BA1.csv')

###### df.head()

-6.1 Plot grade and loan_amnt, grade and annual_inc scatterplot respectively

It is found that there is no obvious relationship between customer grade and annual income and loan amount, so it is only for display and not for analysis.

###### sns.lmplot(x='grade',y='loan_amnt',data=df,fit_reg=False)

###### sns.lmplot(x='grade',y='annual_inc',data=df,fit_reg=False)

-6.2 Scatterplot of grade and rate

Further breakdown shows that customer grade is highly correlated with interest rate. The higher the customer grade, the lower the interest rate. At the same time, this same pattern is observed regardless of the loan term.

### 7. Construct an identification function to predict the ability of the customer to repay the loan

-7.1 Judgment criteria

If the customer's annual income is greater than the median, he/she is considered to have the ability to repay the loan.

###### def J(x,median):return 1 if x>=median else 0

-7.2 Constructing the median

Calculate the median and save the result

###### median_all=df.iloc[:,1:].quantile(q=0.5).to_dict()

###### median_all

-7.3 Output the quantity of customers who meet the criteria

###### df['J']=df.annual_inc.apply(lambda x:J(x,median_all.get('annual_inc')))

###### df

###### print(sum(df.J))
🙌
