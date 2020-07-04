# Analysis of Google Play Store & Apps Store markets to identify popular english free apps
The project is aimed at helping the companies that build free apps to identify the type (or genre) of apps that are most popular on the Google Play Store (Android) markets and the Apple Apps Store (iOS) markets. Also, this project focusses on apps built for English (language) users.

## How can the results be interpreted? And who can use them most effectively?
For several App developing players, 'In-App Advertising' has been and still remains one of the major app monetization tools and a proven revenue model. According to a survey of Apps on Google Play Store by Sweetpricing.com (https://bit.ly/3dBnCFM), about 65% of the Apps on Google Play Store use In-App Advertising for revenue generation.

[![img](https://sweetpricing.com/blog/wp-content/uploads/2016/07/monetization-methods-1280-768x370.png)](https://sweetpricing.com/blog/2016/07/7-app-monetization-stats/)

Thus, for an In-App Advertising Revenue model to work most effectively, the app developers should target an app that will have a large user base. This will make advertising through the app an enticing prospect for the clients of the app developing company. The work in this project identifies the most popular (highest number of users) 'type' or 'genre' of the app across both markets (Google Play Store and App Store). The results of this project can be best applicable to app developing companies looking to launch an app across <b> both Google Play Store and Apple Store and are not restricted by a particular genre/type of app</b>.

## Source of data:
There are over 2 million apps in each of the Google Play Store and the App Store markets. However, for this project, a sample dataset containing few thousands of apps for each market has been used. 

For Google Play Store Apps, a sample dataset containing over 10K apps can be found here: https://go.aws/2Y8Ebm2
For App Store Apps, a sample dataset containing over 7K apps can be found here: https://go.aws/3dHgIPh

For more details of the source for the Google Play Store Apps, please refer to this link: https://www.kaggle.com/lava18/google-play-store-apps
For more details of the source for the Apps Store Apps, please refer to this link: https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps

## Opening the datasets
The first step is to successfully open the datasets and explore them to understand what sort of data is being processed. The first row of both datasets is the header row denoting the type of data in any particular column. In the following cell, the datasets have been opened, the first (header) row has been displayed for both datasets and the first few apps have been displayed just to get a feel of the type of data that will be processed in the project.

## Cleaning Data (Removing dirty data & duplicate entries)
Before the data is analyzed, it is to be ensured that apps with erroneous values or wrong/incomplete values in headers like ratings, price, etc. are excluded from the dataset for the analysis in this project. As per the data source (Kaggle), the 10472 nd entry in the Google Play Store dataset has incorrect/incomplete data elements. After further investigation, it is noted that this entry does not have any value in the 'Category' header and subsequently, other values occupy incorrect (previous) locations in the list/entry. Therefore this entry will not be considered (thus removed in code cell # 2) in the analysis.

It is also desirable to remove the duplicate values/entries to make the results more accurate. This is achieved in a structured way in which, for an App, the entry with the highest number of reviews (thus assumed to be latest) will be retained for the analysis as the correct entry for that particular App.

## Remove Non-English Apps
As mentioned earlier, the scope of the project has been defined to include only English apps. This section removes the non-English apps in the dataset. This is implemented using the ASCII codes for standard English alpha-numeric characters. The 'name' of apps have been used to identify which apps are built using non-English languages or for non-English users. For almost every standard alpha-numeric English character, the ASCII codes are less than '128'. Therefore, a loop to check whether the ASCII value of each element of the string app name is greater than 128 or not has been implemented.

However, it was noticed that certain English apps have special characters like emoticons/emojis, superscript characters, etc. which may have an ASCII value more than 128. 'Instachat 😜' and 'Docs To Go™ Free Office Suite' are two examples of such apps. These apps should not be excluded from the dataset on which the analysis will be done. In order to retain these apps, a check variable (strike_counter) will only exclude an app if there are more than three (03) non-standard characters (ASCII value > 128) in the app name string.

### Isolating Free Apps
The project objective is the analysis of free apps in both markets. In this section, paid/non-free apps are identified using the 'price' header/data in both datasets and excluded from the final dataset to be analyzed.

### Analysing apps based on genre
As per the defined objectives, it is important to recognize free apps that are widely used across <b>both</b> platforms (Google Play Store and IOS Apps Store). The more popular an app is, i.e., the more the number of users, the more ads will be viewed by the users and thus more revenue can be generated by advertising via these apps. Thus, from the datasets, the following data type can be useful for this analysis: Number of users, Category, Rating, Installs, and Genre.

[![img](https://s3.amazonaws.com/dq-content/350/py1m8_family.png)](https://play.google.com/store/apps/category/FAMILY?hl=en)

In the code (cell # 6), the 'genre' data in the Google Play dataset and the 'prime_genre' data in the Apps Store dataset have been analyzed. Based on the genre, a frequency table has been created which throws light on the distribution of the apps across the different genres like 'Games', 'Entertainment', etc. The table indicates a dictionary consisting of tuples that contain the genre name and the percentage of genre apps (out of 100% of the total number of apps). Please note that the categorization of genres is different in the Google Play and App Store datasets.

Result will be represented as:
{(% of genre_1 apps,genre_1 name), (% of genre_2 apps,genre_2 name), (% of genre_3 apps,genre_3 name),.....}
<br>
The frequency tables calculated in code cell # 6 show that the App Store dataset is dominated by 'Games' apps with over 58% of the total apps belonging to this category. On the other hand, the Google Play Store dataset has a more evenly distributed profile for the type/genre of apps with the 'Family' apps constituting about 19% of all apps in the dataset. However, this is not sufficient enough to conclude whether creating an app based on a certain 'genre' is a guarantee for the app to be installed and used by several users. Further analysis of the number of users will give a more complete picture of this project. This will be further explored in the code (cell # 7).

### Analysis of apps based on the number of users
It is important to scrutinize which type/genres of apps have more users than others and which type of apps fare better in terms of popularity in user base in comparison to others. One angle of looking at this is finding the number of users for each genre. However, this approach seems myopic since there may be a high number of apps for a particular genre and therefore a higher user base in total compared to other genres/categories. However, from a more granular perspective, this does not commensurately ensure that a single app of that genre has a higher number of the user base. The parameter that will provide more credentialed insight is the average number of users per app in a particular genre. The code (cell # 7) calculates exactly that.

Please note that in the Google Play the data in the 'Installs' header can be used to estimate the total number of users. However, such a parameter indicating the number of installations is not available in the App Store dataset. Therefore, the data in the 'rating_count_tot' has been used as an approximate figure for the number of users. The approach of a weighted average (average number of users for an app in a particular genre) as a true indication of the popularity of the genre further helps to largely dilute the inaccuracy created due to the unavailability of data. It is thus, a more valid method of actually determining the popularity of a particular genre.

<b>Calcuations used:</b><br>
genre_total_users : Total number of users for a particular 'genre'<br>
genre_total_apps : Total apps for a particular 'genre'<br>
tuple_avg_users_per_app_genre : Average number of users per app for a particular 'genre' ('genre' is the key in tuple)<br>
tuple_avg_users_per_app_genre = genre_total_users / genre_total_apps

### Interpreting the results
The analysis shows the skewed nature of observing pure numbers in certain circumstances. For example, although the 'Games' genre dominates the App Store market with the highest number of apps and consequentially the highest number of users, the average number of users per app is only 14th highest in a list of 23 genres. On the contrary, 'Navigations' apps in the App Store dataset have the 4th least number of users. However, the same genre has the highest number of users per app.

Since the objective of this project is to identify a popular and common type/pattern across <b>both</b> markets, the genre that is particularly popular in both the Google Play Store market and the App Store market is the 'Social' or 'Social Networking' genre. Across both markets, it has a consistent performance of the 3rd highest number of average users per app. Furthermore, in the Google Play Store dataset, this genre has the 6th highest number of user and in the App Store Dataset, it has the 2nd highest number of users. All these factors consolidate the findings of the project that 'Social Networking' apps are a formidable and reliable bet for companies looking to develop apps based on the 'In-App purchase' revenue model. Further analysis of the user ratings of apps and other parameters such as the size of apps can be implemented to gain more insights (or identify any deeper-lying patterns) on the type of apps that are popular across both platforms.
