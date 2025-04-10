# Comcast Telecom Consumer Complaints

This project was completed as a part of assessment for Data Science with Python module. I used different Python libraries such as NumPy, SciPy, Pandas, scikit-learn and matplotlib to complete the given analysis and visualization tasks

## Table of content

1. About The Project [Link Text](#about-the-project)
2. [Objective](#objective)
3. [Data Dictionary](#Data-Dictionary)
4. [Analysis](#Analysis)
  - Task 1
  - Number of complaints per Day
  - Calendar Chart
  - Complaint Types.
  - Number of issue Per Case
  - Topic Model
  - Total Number of Tickets per states
  - Open and Closed Tickets per States
5. [What I Learned](#What-I-Learned)


## About The Project
Comcast is an American global telecommunication company. The firm has been providing terrible 
customer service. They continue to fall short despite repeated promises to improve. Only last month 
(October 2016) the authority fined them a $2.3 million, after receiving over 1000 consumer 
complaints. The existing database will serve as a repository of public customer complaints filed against Comcast. 
It will help to pin down what is wrong with Comcast's customer service. 

## Objective
A project is to make a simple Exploratory Data Analysis to find an insight Comcast is an American global telecommunication company.

## Data Dictionary
The existing [database](https://pages.github.com/](https://www.kaggle.com/code/umairaslam/comcast-telecom-consumer-complaints) will serve as a repository of public customer complaints filed against Comcast.

* Ticket #: Ticket number assigned to each complaint
* Customer Complaint: Description of complaint
* Date: Date of complaint
* Time: Time of complaint
* Received Via: Mode of communication of the complaint
* City: Customer city
* State: Customer state
* Zipcode: Customer zip
* Status: Status of complaint
* Filing on behalf of someone

## Analysis
# Task 1 - Provide the trend chart for the number of complaints at monthly and daily granularity levels
 The first step to convert and get the month and day and by doing that it will make it easier for us to visuailse the results.
 And for that I used this code:-
```
comcast['Date_month_year'] = pd.to_datetime(comcast['Date_month_year']) 
comcast['Month']= comcast['Date_month_year'].apply(lambda x: x.month) 
comcast['Day'] = comcast['Date_month_year'].apply(lambda x: x.day)

```
And now for the monthly graph:-

```
plt.figure(figsize=(9,5))
monthly = comcast.groupby('Month').count().reset_index() #get the data

lp = sns.lineplot(x='Month', y= 'Customer Complaint', data = monthly, sort=False, markers = "o")
aix = lp.axes
aix.set_xlim(0,13) #the limt / range for x-axsis 
aix.annotate('Max complaints in Month (6)', color='red',
            xy=(6, 1060), xycoords='data',
            xytext=(0.9, 0.90), textcoords='axes fraction',
            arrowprops=dict(facecolor='black', shrink=0.1),
            horizontalalignment='right', verticalalignment='top')
```
![image](https://github.com/user-attachments/assets/4778a9ba-00b4-4e89-84a1-13b2e233027f)

The daily graph will have the same idea as monthly graph

![image](https://github.com/user-attachments/assets/867be016-619e-4ef2-bc6c-72364a5831db)

#Task 2 - Provide a table with the frequency of complaint types. 
For this taks I need to find out which complaint types are maximum i.e., around internet, network issues, or across any other 
domains. 

To be able to do the task first I need to get every type and its count.
This is what i used:-
```
service= comcast[comcast['Customer Complaint'].str.contains('service', case = False)]   
scount= service['Ticket #'].count()

```
I used the same code to get the rest of the types.
And here is the result:
![image](https://github.com/user-attachments/assets/e505b765-a989-4f5c-a290-a1ea11be2ff2)

#Task 3 -  Create a new categorical variable with value as Open and Closed. Open & Pending is to be categorized as Open and Closed & Solved is to be categorized as Closed. 
This is a simple one all we need is to create a new column and then use some if-conditions
Here what i used:-
```
comcast["Complaint_Status"] = ["Open" if Status=="Open" or Status=="Pending" else "Closed" for Status in comcast["Status"]]

```
![image](https://github.com/user-attachments/assets/08f2af91-4737-4abf-ba08-05033b13fdda)

#Task 4 - Provide state wise status of complaints in a stacked bar chart
Provide insights on: 
  - Which state has the maximum complaints.
  - Which state has the highest percentage of unresolved complaints.

First we will get the date by using this code:
```
Status = comcast.groupby(["State","Complaint_Status"]).size().unstack().fillna(0)

```
And now to get the stacked bar chart:
```
Status.sort_values('Closed',axis = 0,ascending=True).plot(kind="barh", figsize=(10,9), stacked=True)

```
![image](https://github.com/user-attachments/assets/efd3514d-ff03-456f-ad4c-a54f7e82c52d)

Next is to get the state that has the maximum complaints:
```
comcast.groupby(["State"]).size().sort_values(ascending=False).to_frame().rename({0: "Complaint count"}, axis=1)[:3]
```
![image](https://github.com/user-attachments/assets/edc0cf40-665f-4d3d-891c-3e3c3a6b53c2)

For the state that has the highest percentage of unresolved complaints:

```
un_complaints = comcast.groupby(["State","Complaint_Status"]).size().unstack().fillna(0)
un_complaints['Unresolved_cmp_prct'] = un_complaints['Open']/un_complaints['Open'].sum()*100
un_complaints.sort_values('Unresolved_cmp_prct',axis = 0,ascending=False)[:3]

```

![image](https://github.com/user-attachments/assets/78be8c70-ef8d-459a-a647-4081f128b617)


We find out that **Georgia** has the most complaints and the highest perscentage of unresolved complaints too.

#Task 5 -  Provide the percentage of complaints resolved till date, which were received through the Internet and customer care calls.

To get the percentage of Received Via with Complaints Status i used this code:-
```
compl_pre = comcast.groupby(['Received Via','Complaint_Status']).size().unstack().fillna(0)
compl_pre['resolved'] = compl_pre['Closed']/compl_pre['Closed'].sum()*100
compl_pre['resolved ']

```

Then to visuailse it by using Pie chart:

```
lbl= 'Customer Care Call', 'Internet ' 
plt.pie(compl_pre['resolved'], labels=lbl,startangle=90 ,autopct='%1.1f%%')
plt.title('Received Via Percentage')
plt.legend(loc="lower right")

```

![image](https://github.com/user-attachments/assets/a2f44eb9-458c-4f58-8648-b0146428da9f)

##What I Learned
- Understand the importance of Business Analytics.
- Understand the importance of Data Visualization and how to implement it.
- Understand more about Python.
- Understand new Python functions.
- Reading and Search skills.


