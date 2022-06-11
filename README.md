<h1>Weather Effects on Mental Health</h1>

<h2>Motivation</h2>
Within the field of psychology, there has been a lot of research conducted on if the weather can influence people’s moods. For individuals living in climates with varying climates, the periodical changes occur from warmer to colder seasons include fewer hours of sunlight, colder temperatures, and more overcast skies. One finding from previous research has found that increased sunlight and decreased wind lead to less negative feelings amongst many individuals (Grohol). 

<br/>As a student studying in a technology field, I am curious whether changes specifically affect people working within the field of technology. Additionally, a previous study found that women are much more responsive to weather fluctuations than males (Connoly). I was intrigued by this finding and wanted to investigate it to see if the same pattern occurs within our data. The goal of my project is to examine and better understand the patterns between weather and geographic location with the mental health of people working in the tech field.

<h2>Data</h2>

<h3>Data Set 1: Open Weather Org</h3>
The first data source I have used for this project was the Open Weather Org (https://openweathermap.org/api) which allows me to access the current weather for any location including over 200,000 cities. I chose this source to gather information about weather across the United States because it offers weather information for a multitude of locations and it provides very detailed information about many elements of weather. I decided to use the city capital of each of the states in the United States as my 50 locations so I could get a mix of different climates. 

<br/>Before I made the request to the API, I created a state capitals dictionary (capital_dict) which contains each state as the key and the corresponding capital city as the value. I also made a state abbreviation dictionary (us_state_abbrev) which has each state as the keys and the corresponding state two-letter code as the value. The state capital dictionary allows me to only get weather information from those 50 cities. The state abbreviation dictionary was necessary to make because I needed the states in that format so they matched the state format of our other data source.

<br/>I looped through my state capitals dictionary and for each state in the dictionary I made a request to the Current Weather Data API using the get() method. I then converted the response into JSON format where I was then able to use indexing to grab the specific information that I was interested in. The information I was specifically interested in gathering from this source were temperature (float), description of the current weather (string), wind speed (float), humidity (float), hours of daylight (int), state (string), state code (string), and city (string). For each iteration of the for loop, I appended the data in a tuple format from the Current Weather Data API for that specified city and state to an empty list (temp_data). Using all the information I gathered and added to our temp_data list, I then created a dataframe (temp_df) containing all of this data.

<br/>![Data Source 1](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Data%20Source%201.png "Data Source 1")
<h4 align="center">Figure 1: Reading Open Weather API Data</h4>

<h3>Data Set 2: Mental Health for People Working Technology</h3>
The second data source I used is a CSV document through Kaggle (https://www.kaggle.com/osmi/mental-health-in-tech-survey) that contains data from a mental health survey that 1,433 adults living in the United States who work within the field of technology took in 2014. I chose to use this data source because it is free to access, focuses on people working in the field of technology, and it contains numerous measures of an individual's mental health. After downloading the CSV file from Kaggle with the data and uploading it to my Jupyter Notebook workspace, I used the Pandas read_csv() function to read in the file and convert it to a pandas DataFrame. 

<br/>The variables that I thought were interesting were treatment (string) (denotes whether or not they sought mental health treatment), family history (string), remote work (string), and age (string). I wanted these other variables in addition to treatment because I believed that these would be worthy to examine if there was any correlation between the variables from the same dataset. For example, it would be interesting to see if remote work has had an impact on mental health.

<br/>![Data Source 2](https://github.com/goel-mehul/Predicting-Soccer-Player-Ratings/blob/main/Images/Null%20Values.png "Data Source 2")
<h4 align="center">Figure 2: Reading Kaggle Data</h4>

<h2>Data Processing</h2>

Before any data analysis could be done, I needed to clean and organize our datasets for examination.
* I first converted our temperatures from Kelvin to Fahrenheit to make it easier for someone to read and understand our datasets. To do so, we created a function convert_to_fahrenheit.
* After doing so, I also wanted to find the amount of sunlight received every day. Since I had sunrise and sunset values in epoch format, I decided to create and use another function, convert_to_hours, which gave me the difference in sunrise and sunset for each row in terms of hours. Based on all this collected data, I ran API calls and built a pandas dataset called temp_df.
Apart from the APIs, I used another previously created dataset and read that through a CSV file. This dataset also had many issues and needed cleaning before it could be utilized.
* I first cleaned the gender column. Rows had different values that were meant to show the same responses. For example, ‘Male,’ ‘male’ and ‘M’ were all used to represent the same gender category. Thus, the gender column was cleaned to represent only one of these values in the dataset.
* Apart from this, there were some errors present in the Age column that needed to be fixed. Values like 300, 999999, and -20 for age were removed as these ages are not possible and the data was clearly faulty. This was similarly done for other values in this column by using lambda functions. Once the Age column was cleaned, I was done with my data cleaning.

Since I decided to focus the study in the US, only rows where the participant was in the US were selected to be kept in the dataframe. This new dataset was stored as df_state.
Once the two datasets were cleaned and ready to use, I merged them on state codes. I merged them on inner join as I wanted to make sure that the entries were present in both datasets.

<br/>![Merging Datasets](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Merging%20Datasets.png "Merging Datasets")
<h4 align="center">Figure 3: Merging Datasets</h4>

<br/>![Sample Query](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Sample%20Query.png "Sample Query")
<h4 align="center">Figure 4: Sample Query</h4>

<h2>Analysis and Visualization</h2>


<h3>Temperature vs. Sought Treatment</h3>

By creating a linear regression model using the data between the percent who had treatment and the average temperature for each state, one can see that there is no statistical correlation present between the two variables. This is surprising when compared to some of the research done in this field. According to recent research, the authors concluded that cooler temperatures decrease the level of adverse mental health outcomes and that warmer temperatures increase negative health outcomes. (Chan). Thus, this data is not consistent with previous research and findings.

<br/>![Temperature-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Average%20Temperature%20vs%20Treatment.png "Temperature-Treatment Graph")
<h4 align="center">Figure 5: Temperature vs Sought Treatment Graph</h4>

<h3>Statistical Testing: Temperature vs. Sought Treatment</h3>

I found that the slope of the line representing their relationship is 0.16 which indicates a very weak positive correlation between the two variables. The root means square error value is 25.75. The R-squared value is 0.0039 which shows that the regression model I created does not fit the observed data well. Only 0.39% of my data fits the regression model so it is not an accurate model for the data at all. The main takeaway from this linear regression was that there is not a significant relationship between the average temperature and whether tech workers sought treatment for their mental health. Variances in the temperatures does not have an effect on whether tech workers get mental health treatment.

<h3>Hours of Sunlight vs. Sought Treatment</h3>

By creating a linear regression model using the data between percent who had treatment and the average sunlight per state, one can see that there is a moderate positive correlation between the two variables. It was found that decreases in sunlight exposure can have significant negative effects on both moods and can even cause cognitive impairment (Kent). Not only that, another source by Psych Central states that “greater amounts of sunlight … decreased these negative feelings” (Grohol). Thus, this data is consistent with previous research and findings.

<br/>![Sunlight-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Average%20Sunlight%20vs%20Treatment.png "Sunlight-Treatment Graph")
<h4 align="center">Figure 6: Sunlight vs Sought Treatment Graph</h4>

<h3>Statistical Testing: Hours of Sunlight vs. Sought Treatment</h3>

This test found that the slope of the line representing their relationship is 17.59 which indicates a strong positive correlation between the two variables. As the hours of sunlight increase, the number of workers who got mental health treatment also increases. This is not what I had initially expected. I thought that fewer hours of sunlight would lead to more people seeking treatment. The root means square error value is 24.43. The R-squared value is 0.104 which shows that 10.4% of the data fit the regression model I created. This is a small percentage which means the regression model is not that accurate.

<h3>Wind Speed vs. Sought Treatment</h3>

I created a linear regression model using the data to compare the percent of tech workers who had treatment and the average wind speed for each state. I found that there is a negative correlation present between the two variables. There isn’t much research available to document the relationship between wind speed and mental health problems. There is one research study related to this topic but it focuses on wind direction and mental health problems (Bos). Thus, no satisfactory outcome can be achieved.

<br/>![Wind Speed-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Average%20Windspeed%20vs%20Treatment.png "Wind Speed-Treatment Graph")
<h4 align="center">Figure 7: Wind Speed vs Sought Treatment Graph</h4>

<h3>Statistical Testing: Wind Speed vs. Sought Treatment</h3>
The test found that the slope of the line representing their relationship is -4.75 which indicates a moderate negative correlation between the two variables. This can be interpreted as when wind speeds decrease, the number of tech workers who have sought mental health treatment increases. The root means square error value is 24.68. The R-squared value is 0.085 which shows that the linear regression model I created does not fit the observed data well, because only 8.5% of the data fits my regression model. The linear regression model does not predict the actual values of the actual data well.

<h3>Humidity vs. Sought Treatment</h3>

I created a linear regression model to compare the average humidity vs. the percentage of people who had treatment. The model found that there is not any correlation between the two variables. However, while researching the topic, it was found that “compared with temperature, the humidity had a stronger impact on the negative impact on mood” (Park). Therefore, it was surprising for me to not see any correlation or relationship between the two variables.

<br/>![Humidity-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Average%20Humidity%20vs%20Treatment.png "Humidity-Treatment Graph")
<h4 align="center">Figure 8: Humidity vs Sought Treatment Graph</h4>

<h3>Statistical Testing: Humidity vs. Sought Treatment</h3>
This test found that the slope of the line representing their relationship is -0.29 which indicates there is essentially no correlation between the two variables. The average humidity of a place does not influence whether tech workers got mental health treatment. The root means square error value is 25.17. The R-squared value is 0.053 which shows that the linear regression model we created does not fit my observed data well, because only 5.3% of the data fits my regression model. The linear regression model does not predict the actual values of the actual data well.

<br/>![Linear Regression Code](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Linear%20Regression%20Function.png "Linear Regression Code")
<h4 align="center">Figure 9: Linear Regression Code</h4>

<h2>What Didn’t Work and Why</h2>
Reasons why the code might have not worked as intended:
* Most of the data related to mental health were collected through surveys. Due to this, there were some survey results that had wrong information present. People had marked their ages as 300 or -20. There were other ways that the data present was bad. Thus, the analysis may not be completely accurate due to the presence of faulty data.
* Furthermore, for some of our variables, I had a limited range of data points. For example, when finding the correlation between average hours of sunlight and treatment sought, most of the data points recorded had 13 hours of average sunlight. Due to this any data point with 11 or 12 hours of average sunlight would change the slope of the curve.
Thus, the analysis of hours of sunlight vs treatment sought may not be completely accurate.

<h2>Additional Interesting Comparisons & Visualizations</h2>

<h3>State vs. Sought Treatment</h3>

I created a bar plot to analyze the percentage of people who sought treatment when compared to each state. However, there is not a lot of information one can get for an analysis. Louisiana, Maine, Mississippi, and Idaho had the highest percentage of people who sought treatment.

<br/>![State-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/State%20Vs%20Treatment.png "State-Treatment Graph")
<h4 align="center">Figure 10: State vs Sought Treatment Graph</h4>

<h3>Age vs. Sought Treatment</h3>

I created boxplots to compare people’s age to those who sought mental health treatment. Based on the data present, there is no evidence to suggest any correlation between age and those who sought treatment.

<br/>![Age-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Age%20Vs%20Treatment.png "Age-Treatment Graph")
<h4 align="center">Figure 11: Age vs Sought Treatment Graph</h4>

<h3>Remote Work vs. Sought Treatment</h3>

I created a bar plot to count the number of times someone has sought treatment whether or not they were working remotely or not. Those who were not working remotely had a larger number of treatment cases, however as a percentage, those who worked remotely reported more number of treatment cases. This may signify that people who work remotely, may feel alone and due to loneliness, may develop mental health problems compared to those who work in offices. This could mean there may be a positive correlation between working remotely and developing mental health problems.

<br/>![Remote Work-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Remote%20Work%20Vs%20Treatment.png "Remote Work-Treatment Graph")
<h4 align="center">Figure 12: Remote Work vs Sought Treatment Graph</h4>

<h3>Gender vs. Sought Treatment</h3>

I created a bar plot to count the number of instances of mental illness between genders. For people who identified as men, there was a pretty even spread between Yes or No, with around 250 people responding ‘Yes’ and ~270 people responding ‘No.’ On the other hand, many more women on average responded ‘Yes’ with around ~120 people responding ‘Yes’ and ~50 people responding ‘No.’ Men are generally known to be less vulnerable and accepting of their own mental health issues, while women are known to be much more honest and empathetic with their own and other people’s emotions than men. In this case, many men may actually have a mental illness but refuse to seek treatment or report the fact that they do.

<br/>![Gender-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Gender%20Vs%20Treatment.png "Gender-Treatment Graph")
<h4 align="center">Figure 13: Gender vs Sought Treatment Graph</h4>

<h3>Family History vs. Sought Treatment</h3>

I created a bar plot to count the number of times someone has sought treatment with whether or not they have had a family history of mental illness. Not surprisingly, if the employee did not have a past family history, they did not report having sought treatment. On the other hand, if they did have a family history of mental illness, there were much more people who sought treatment. This could signify that since people are more aware of their mental illness and want to be proactive about their potential mental health problems. This could also mean that there is a link between there is a positive correlation between family history of mental illness and a person’s mental illness.

<br/>![Family History-Treatment Graph](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Family%20History%20Vs%20Treatment.png "Family History-Treatment Graph")
<h4 align="center">Figure 14: Family History vs Sought Treatment Graph</h4>

<h2>Tests</h2>
I created a TestSum class to test if all the functions I created were working as I expected them to. My code passes all of the tests so I am confident all the code I wrote is working successfully and as expected.

<br/>![Tests Code](https://github.com/goel-mehul/Weather-Effects-on-Mental-Health/blob/main/Images/Tests.png "Tests Code")
<h4 align="center">Figure 15: Tests Code</h4>

<h2>Citations</h2>
Bos, Elisabeth Henriette, et al. “Wind Direction and Mental Health: a Time-Series Analysis of Weather Influences in a Patient with Anxiety Disorder.” BMJ Case Reports, BMJ Publishing Group, 8 June 2012, www.ncbi.nlm.nih.gov/pmc/articles/PMC4543179/.

Chan, Emily Y Y, et al. “Association between Ambient Temperatures and Mental Disorder Hospitalizations in a Subtropical City: A Time-Series Study of Hong Kong Special Administrative Region.” International Journal of Environmental Research and Public Health, MDPI, 14 Apr. 2018, www.ncbi.nlm.nih.gov/pmc/articles/PMC5923796/.

Connolly, Marie. “Some like It Mild and Not Too Wet: The Influence of Weather on Subjective Well-Being.” Journal of Happiness Studies: An Interdisciplinary Forum on Subjective Well-Being, vol. 14, no. 2, Apr. 2013, pp. 457–473. EBSCOhost, doi:10.1007/s10902–012–9338–2.

Grohol, John Psy. D. M. “Can Weather Affect Your Mood?” Psych Central, 29 Aug. 2014, psychcentral.com/blog/can-weather-affect-your-mood#1.

Howarth, E., and M. S. Hoffman. “A Multidimensional Approach to the Relationship between Mood and Weather.” British Journal of Psychology, vol. 75, no. 1, Feb. 1984, pp. 15–23. EBSCOhost, doi:10.1111/j.2044–8295.1984.tb02785.x.

Kent, Shia T et al. “Effect of sunlight exposure on cognitive function among depressed and non-depressed participants: a REGARDS cross-sectional study.” Environmental health : a global access science source vol. 8 34. 28 Jul. 2009, doi:10.1186/1476–069X-8–34

Park, Kyung-Soon & Lee, Soogeun & Kim, Eunjeo & Park, M. & Park, Jeeyeon & Cha, M.. (2013). Mood and weather: Feeling the heat?. Proceedings of the 7th International Conference on Weblogs and Social Media, ICWSM 2013. 709–712.
