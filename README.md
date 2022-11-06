# EDA and Model Building on PAMP2 Dataset

For this project the PAMAP2 data set is used. PAMAP2 data is the data of physical Activity Monitoring. The data set contains recorded information from 18 activities performed by 9 subjects, wearing 3 IMUs and a HR-monitor. The activities performed by the subjects comprises of a wide range of everyday, household and sport activities. The goal is to derive actionable insights to which will help in developing hardware or software which can determine the amount and type of physical activity carried out by an individual.

# Details of the data used:

IMU - An IMU is a type of sensor that measures angular rate, force and sometimes magnetic field. IMUs are composed of a 3-axis accelerometer and a 3-axis gyroscope, which would be considered a 6-axis IMU. IMU also includes an additional 3-axis magnetometer, which would be considered a 9-axis IMU. Technically, the term “IMU” refers to just the sensor, but IMUs are often paired with sensor fusion software which combines data from multiple sensors to provide measures of orientation and heading. In this experimet 3 Colibri wireless IMUs are used, which have sampling frequency of 100Hz each. 1 IMUis placed over the wrist on the dominant arm, 1 IMU is placed on the chest and 1 IMU is placed on the dominant side's ankle.

Heart Rate Monitor - Heart rate monitors work by measuring electrical signals from the heart in real time.In this experiment 1 Heart Rate Monitor is used by each subecjt which has a frequency of 9Hz.

Suject Information - There are 9 subjects who have participated in the experiment. The subjects are mainly employees or students at DFKI. There is 1 female, 8 males the subject aged between 27.22 ± 3.31 years and BMI 25.11 ± 2.62 kgm-2. Each of the subjects had to follow a protocol, containing 12 different activities.

Altogether, over 10 hours of data were collected, from which nearly 8 hours were labeled as 1 of the 18 activities performed during data collection


There are 9 dat files for each subject which contains the real time data of each subject, the activities they performed, the IMU readings and HR-monitor readings. I have converted the dat files into csv files before loading the files. This will ensure all the information is stored in 1 data set and will be easily available when required. The subjects have been given the subject ID's. A list of the file names has to be created in order to load all the files and create the data frame. Moreover, a dictionary that will hold the names as well as numbers of each different activity has to be created in order to be able to understand which activity is being analysed at each phase. Lists for each different category of IMU's have to be put together as well in order to have the column names for the data frame. IMU's that will be used are for chest, ankle and hand. Then all the different lists have to be put together to create the collection of the columns. There are total 55 columns in the data set.

# Exploratory Data Analysis

To get the maximum insights from data performing Exploratory Data Analysis is essential. The main purpose of EDA is to detect any errors, outliers as well as to understand different patterns in the data.

Activity 0 is a transient activity.The data related to this activity mainly covers transient activities between performing different activities, e.g. going from one location to the next activity's location, or waiting for the preparation of some equipment. Also, different parts of one subject's recording (in the case when the data collection was aborted for some reason) was put together during these transient activities.Therefore, I have removed data with activity ID 0 as it contains transient activity which is not required for the data analysis.

Before moving into further exploratory data analysis , checking if there are still any missing values in the data. As we can see the heart rate column still has 4 missing values. The reason why heartrate still has NaN values is because interpolation calculates the values around the NaN cell. Since the first cells are NaN it is normal to generate new NaN values after interpolation. To overcome this problem we can assume that the value of the first 4 cells is 100 since the values after the index 4 is 100. Doing so will eliminate any NaN values from our dataset.

![image](https://user-images.githubusercontent.com/103538049/200192413-a0612970-1b33-4be0-80d8-74ba2608a9dd.png)


In order to carry the analysis and then testing the analysis, I have divided the data into train and test data sets. The training data will contain 80% of the data, while the testing data will contain 20% of data. In the below cells we can see the Data frame for training_data and testing_data

Next,  I have grouped the data with respect to the subject ID's, activity ID and timestamp. The idea is to check the amount of activities perfromed by each subject and to check if the data is balanced. As we can see from the graph below suject ID 9 has performed the lowest activities and thus we cannot use subject ID 9 to perform any analysis as we dont have much data recorded from subject 9. On the other hand subject ID 5 have performed highest range of activities followed by subject ID 2. We can say here that the data is not 100% balanced.

![image](https://user-images.githubusercontent.com/103538049/200192478-930d34eb-c3ca-4302-ba09-d5c859533199.png)


For the next analysis, I have grouped the data set with respect to the heartrate and Activity ID. The idea is to check if all the activities recorded similar heart rate or if there is major difference between the heart rates while performing different activities. As we can see from the graph below activities like running and rope jumping have the highest heart rate and activities like ironing have heart rate lower than others. Few of the activities like watching TV or Computer work are not in the graph as the subjects did not perfrom them.


![image](https://user-images.githubusercontent.com/103538049/200192537-a83405a7-cd79-4271-b41e-7b99885d6a9c.png)


Next, I have grouped the data with respect to subject ID and heartrate to check which subject ID had the highest heart rate. As we can see subject ID 9 had the highest average heart rate and subject ID 9 has the lowest heart rate.

![image](https://user-images.githubusercontent.com/103538049/200192572-6ecbf175-8448-4d7d-a6e3-d6448b07c957.png)


We are going to focus on heart rate as it is our most precice meter of check for tracking subjects during activities as implied by the various indications on the readme file of the dataset. With that in mind, looking at the table, we can observe that the mean heart rate throughout the dataset is 107.4 . Furthermore the minimum heart rate is 57 and the maximum heart rate is 202. The quartiles that are shown can be further analysed by plotting a box plot which will help with understanding our outliers and quartiles groups and also shown the mean of our data's heart rate.

![image](https://user-images.githubusercontent.com/103538049/200192609-01258e7d-11f3-41cd-b0c3-97c4e8d6b6c8.png)


Looking at the box plot, we can see that the outliers have heart rate from 180 up to 202. Our highest quartile group out of the four starts from 124 which is the end of the Inter-quartile range and finishes at 180 which also makes it our biggest group by looking at the size of it on the box plot compared to the other quartiles. Meaning that the biggest amount of subjects on the activities performed had heart rate of 124 up to 180. Our third quartile group starts from the mean value which is 107.4 which is where the horizontal line in our box is, and finishes at the end of the Inter-quartile range which is 124. Our second quartile group, starts from the start of the Inter-quartile range which is 86 and ends at the mean value 124. Our first quartile group starts from the lowest data point, 57 and ends at the start of the Inter-quartile range 86. Our box plot also shows that most subjects performed some activities at a statistically similar way but failed to do the same in all activities which explains the big upper quartile group.


Next,  I have grouped the data with respect to heart rate, activities, hand temperature, chest temperature and ankle temperature. This will give us insight if there is any chnage in the temperature of the body while performing the activities. As we can see from the graph below, there is not much difference in the temperature level for hand, chest and ankle during all the actvities , except we can see a slight(negligible) drop in temperatures for Activities - Running and rope jumping.


![image](https://user-images.githubusercontent.com/103538049/200192685-97d10e25-894d-43ea-b181-8a5fca3dd2d2.png)


 Next,I have grouped the data with respect to activity ID, heart rate , ankle Manget, hand magnet and chest magnet readings. As we can see from the graphs ironing, vaccum cleaning, Running, cycling , rope jumping have a better reading of ankle, hand and chest magents. This is because while doing these activities there were movement of hands, ankles and inturn the heart rate.
 
 ![image](https://user-images.githubusercontent.com/103538049/200192749-bcf9bb17-eb2f-4f6b-81de-a393f3726b55.png)


To check further on our data to see any anormalities, we have to plot a heat map which will show whether our data has correlations inbetween it. All columns will be used in order to understand the extend of problems, if there are any.

Our heatmap shows how much statistical similarity there is between our different columns. We can every easily observe that the gyroscopes do not correlate with any of our other data and seem unneeded in this model. On the other hand we can understand the correlation between accelerometers of the hand and temperature. The two are strongly correlated on all three instances of hand accelerometers. Furthermore the chest Magnetometers seem to be correlated with heart rate and it is very logical as they very close together on the body.


![image](https://user-images.githubusercontent.com/103538049/200192793-d9e21bb0-f1ad-482d-abae-66d31635c15b.png)


Next I have created a new Data Frame "Subject1df" which includes all the columns from training data. From this data frame we will get the values of activities performed by each user in the table. For the subjects who have not performed the activities, the value is replaced by 0. In this way we can see which subject perfromed which type of activities and try to see the differnece.


![image](https://user-images.githubusercontent.com/103538049/200192837-aad48645-2f25-4ee5-ba29-db884a42298c.png)

From the "subject1df" created above I have grouped few subjects with respect to heartrate, timestamp, hand , chest and ankle acceleration. Below I will be focusing on the effect of hand acceleration required for each type of actvity and the variation of heart rate associated with it.

To find the total hand acceleartion I have used formula - sqrt(HandAcceleration_1 ^2 + handAcceleartion_2 ^2+handAcceleartion_3 ^2)

To find the total chest and ankle acceleration I have added the values toghether.

Next I have added 3 new columns to this data to store the total acceleation values. The column names are total_hand_acceleartion, total_chest_acceleartion and total_ankle_acceleartion. For analysis purpose I will be comparing the heart rate with total hand acceleartion.


![image](https://user-images.githubusercontent.com/103538049/200192875-5736b550-e1d0-4c3f-8444-51195513654b.png)


For the next steps of Analysis, I have compared each subjets heart rate for the respective activities. 

Here I have grouped subject1's heart rate and hand acceleartion, chest acceleartion and ankle acceleration with respect to the 16th activity which is Vaccum Cleaning. As we can see from the graph there is constant fluctuaion in the heart rates and the hand, chest and ankle acceleartion as Vaccum cleaning involves moving from one place to another, using the hands while using the vaccum cleaner. THis suggests that Vaccum Cleaning is tracked properly by the IMU sensors and the HR monitor

![image](https://user-images.githubusercontent.com/103538049/200192958-4287d205-72cf-47a1-b2bf-0024bb95b80a.png)

![image](https://user-images.githubusercontent.com/103538049/200192974-99b1a603-27a9-4c4c-a141-7b8b3fa97bf1.png)


Here I have grouped subject1's heart rate and hand acceleartion, chest acceleartion and ankle acceleration with respect to the 5th activity which is running. Running involes a lot of physical cumbersome activity and it is clear from the above graphs that as the subject 1 is running there is a steady increase in the hand acceleartion, chest acceleartion and highest increase in ankle acceleration. The heart rate is also very high during this activity.

![image](https://user-images.githubusercontent.com/103538049/200192994-846233f0-4e5b-49fe-82e5-b57689746134.png)

![image](https://user-images.githubusercontent.com/103538049/200193006-dd37fc4b-cf12-4434-ba0b-e2a67ce0991d.png)


From the above data analysis it is clear that the heart rate is dependant on the total hand chest and ankle acceleation required to perform the activity. If the activity involves a lot of physical movements of hands and ankles the heart rate will be more. If the activity does not involve much physical movemnets, the heart beat will be less. Analyzing this , my conclusion is that cumbersome activities have more heart rate and acceleartion of hands, chest and ankle as compared to activities like lying, standing, ascending stairs etc

# Hypothesis
Based on the above data analysis the main focus of testing will be the acceleartion of hands, chest and ankles and the heart rate associated with it. To prove the fact that more the accelartion, more the heart rate, I will be focusing on the heart rate feature more.

Below is the hypothesis that I will be testing

H0 - Activities with less Acceleration of hands and ankles have the same heart rate as that of other cumbersome activities

H1 - Activities with more Acceleration of hands and ankles have more heart rate than the activities with less Acceleration of hands and ankles.

IN the cell below, I have created "CumbersomeActivities_data" which contains the data of activties - running, rope jumping, descending stairs and another set of data "Convinient_data" which contains the data of activities - descending stairs, nordic walking, and walking. I will be comparing the average heart rates of these 2 values to validate the hypothesis. As we have earlier proved that activities with high acceleartion have high heart rate I am only using heart rate feature to test the hypothesis.

![image](https://user-images.githubusercontent.com/103538049/200193097-c0c2bd04-d393-48a2-aaf4-733f522b705f.png)

To make the distribution of data even I have taken random samples of data from both CumbersomeActivities and Convinient_data and extracted the heartrate for each subject for the respective activities. After taking the samples I have stored them in RopeMean and lyingMean and took a collective mean of the data to perform the z-test. As we can see from the graph below the data we have converted the data into a standardized normal curve and now the mean and standard deviation are evenly distributed from both the sides before performing the z test.

![image](https://user-images.githubusercontent.com/103538049/200193125-e339e05d-bd0b-4308-8937-26b2fefc5b78.png)


![image](https://user-images.githubusercontent.com/103538049/200193151-c4b5419b-0c22-4720-a57f-dcc06a92fc0f.png)


![image](https://user-images.githubusercontent.com/103538049/200193172-eafac2c8-414e-4d13-b28a-2d22c2c160ab.png)


As the p-value is 0, we reject the Null Hpotheisis (H0) - Activities with less Acceleration of hands and ankles have the same heart rate as that of other cumbersome activities and accept the alternate hypothesis (H1) - Activities with more Acceleration of hands and ankles have more heart rate than the activities with less Acceleration of hands.

This confirms that if there is considerable movement of hands, shoulders, ankles while performing any activity the heart rate will increase and the sensors will track the real time data.


# Modeling

Data Modelling is the process of analysing the data objects and their relationship to the other objects. It is used to analyse the data requirements that are required for the business processes. These models are created for the data to be stored in a database. The Data Model's main focus is to figure out what operations we have to perform. I will be exploring 2 models and check for the accuracy to interpret which model fits best for this data.

Some variables have to be dropped which would impact our modelling precision. The variables to be dropped are timestamp and subject_id as they are numeric numbers which would our modelling method would use in its calculations but since their values don't have any meaning, the modelling method used would have noise and predictions of accuracy would be inaccurate.

![image](https://user-images.githubusercontent.com/103538049/200193241-e9a82ef2-30fa-4914-86c7-9055bbea6cc0.png)


# PCA
Principal Component Analysis, or PCA, is a dimensionality-reduction method that is often used to reduce the dimensionality of large data sets, by transforming a large set of variables into a smaller one that still contains most of the information in the large set. The aim of this step is to standardize the range of the continuous initial variables so that each one of them contributes equally to the analysis. Organizing information in principal components will allow to reduce dimensionality without losing much information.

Usually 90-98% of the variance will explain our data really well. So by plotting the variance ratio aginst the number of componments we could see how many of those we could use. As we see from the graph below 15 componments fall around to 94% of the variance.

![image](https://user-images.githubusercontent.com/103538049/200193302-6e482f1f-4683-4e80-a1fe-dac3f527d462.png)

After performing PCA on the data now I will be exploring 2 moddeling techniques. i.e. Logistic regresseion and Linear Regression.

Logistic Regression - Logistic Regression is a Supervised learning algorithm that can be used to model the probability of a certain events. The result of it is a probability that a data point is part of a class. Here we will train our training model into classification to calculate the probability , recall and F1 score and then cross validate the results on testing data set.

Linear Regression - Linear Regression is the process of finding a line that best fits the data points available on the plot, so that we can use it to predict output values for inputs that are not present in the data set we have. In the below cells we have calculated the probability of the the output given certain inputs.


![image](https://user-images.githubusercontent.com/103538049/200193348-a4ec7be6-d885-4ca2-b329-de1e733a7763.png)


As we can see from the above output the accuracy of the training data set is about 0.53, Error rate is 0.53. The Recall and F1 scores are low than expected.

In the below cell we will be exploring the linear regression algorithm. To start with, first we have to find the intercept and co-effients of the data and then put those values in the predictor which will give us the predicted output values.


![image](https://user-images.githubusercontent.com/103538049/200193385-4dc8e4e5-31cd-4586-9b49-846c7344ef64.png)


![image](https://user-images.githubusercontent.com/103538049/200193404-33267bb5-81ef-4b8d-a6c2-04b29dae6902.png)


As we can see from the above output of table and the graph , the precidted values are much higher than the actual vaules and here for our Model Linear Regression is not the best fit.


# Cross Validation

Even though the above models seem to perform really good, the metrics used for that do not represent the real score since the models were train on a specific part of the dataset. By using cross validation, we could k=10 number of folds, which in few words, will generate 10 different samples. By doing that we will get 10 different metrics values. The mean value of these metrics will show a better representation of our model's performance.

![image](https://user-images.githubusercontent.com/103538049/200193476-2f05d0e4-99f2-4256-9339-9613391cd2fe.png).


From the cross verification logistic regression can be considered to be a better fit for the model than linear regression. There is 53% of accuracy and we can improve the accuracy further by performing Hyperparameter Tuning - Grid Search. There is some amount of class imbalance n our data which can be explored more and then removed before performing the classification.

# Conclusion
From the above Analysis we have proved that the activities with more physical movements will lead to more acceleartion of hands, chest and ankle which in turn will increase the heart rate. The linear regression model used to check the accuracy did not fit perfectly on the data. Hence, I have used Logistic Regression model which gave accuracy of 53% which can further improved by Hyperparameter Tuning - Grid Search.











