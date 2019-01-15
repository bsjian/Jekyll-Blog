---
title: "Housing Recommendation System - Report"
layout: post
date: 2018-12-03 23:18
image: /assets/images/markdown.jpg
headerImage: false
tag:
    - Data Visualization
    - CSE6242
    - Report
category: blog
author: boshengjian
description: CSE6242 Report 
---

## Summary

See the PDF version [here](https://drive.google.com/file/d/1GATi0CzKwm3YkMevMGeomTWYtWJsAdzW/view?usp=sharing).

See the code on [Github](https://github.com/bsjian/CSE6242-Team-Aha-Project), and the explanition [here](http://www.boshengjian.com/CSE-6242-Team-Aha-Project/)

Check out the [demo](https://afternoon-citadel-68589.herokuapp.com/)

This is the final report for CSE6242 Data and Visual Analytics.

---

## Contents
- [Introduction-and-Problem-Definition](#introduction-and-problem-definition)
- [Literature-Review](#literature-review)
- [Proposed Methods](#proposed-methods)
- [Evaluation](#evaluation)
- [Conclusion](#conclusion)
- [Reference](#reference)

---

## Introduction and Problem Definition

Nowadays, more than 90 percent people use web recommendation systems to buy or sell their real estates [1,2]. There are plenty of platforms which integrate the real estate data and present them in one website, like Google, Yelp, Zillow [3].

However, none of these current systems is designed for meeting all needs of college students and daily commuters in one place. Those people, when looking for housing, need to search among lots of different websites to gain information they need. For example, one might need to search for houses on Zillow, then move to Google maps for transit time and then look up reviews of leasing offices or agencies. Furthermore, one might have additional preferences such as shopping location, favorite store and dog parks.

So we want to build a housing rental recommendation website for those who commutes daily, which can provide renting recommendations based on users’ personal references, such as basic information, safety anticipation, and transportation requirement. 


---

## Literature Review
 
Various attributes were found to be common when people search for a house to rent: the property location, property type, rent, number of bedrooms, number of bathrooms, the area of living space, availability of parking space, whether they are furnished or serviced and the daily commuting distance and time. Many researches have been done to analyze the relationship between transportation and other metrics based on renters’ choices of renting [4-7]. Researches also show that the renters’ choices are also highly related with their occupations. Those researches show how we may improve our project in the future, which is using the date came from our own website to build model and improve the recommendation system. 

At the same time, crime rate is an important factor when it comes to residential decision. It is known that people would prefer to live in safer areas, even if it’s more expensive. Researchers used a two-stage residential sorting model and the results showed that people are willing to pay more to move to a location with lower violent crime occurrences [8,10]. However, it’s hard for us to quantify the extent as we don’t have dataset of how much more money people are willing to pay for an apartment with lower violent crime. 
Also, it is undeniable that crime rate and house price are innately correlated. George et al used instrumental variable (IV) method to analyze the relationship of crime rate and land price in Japan and concluded that every 10% decrease in burglaries rate would result an average 1% rise in land price [11]. Given that it was conducted in Japan, we still want to use 
the same method to analyze the relationship (between crime rate and house price) in Atlanta. As for crime types, researchers have differentiated property crime and violent crime by using hedonic regression, and also quantified the intangible cost of crime [12-14]. The results differed from poor, middle and rich neighborhoods and, violent crime is the dominant factor affecting house prices. Thus we aim to only focus on violent crimes in Atlanta. 

Our system uses multiple datasets from different sources, including user input data and verified or unverified public datasets. Gelman et al points out that plenty of information input by end-users are inaccurate [14]. The author uses data event analysis and error cluster analysis for investigating errors. Data event analysis involves a review of all the processes that capture or manipulate the data and error cluster analysis centers on a data subset that contains data suspected of being incorrect. We plan to use similar methods to verify inaccurate data and clean inaccurate data. 

As for recommendation system, Verma et al shows how to build recommendation system with Hadoop and utilizes recommendation algorithms in Mahout Architecture [15]. To solve problems of natural language ambiguity, Manikrao et al proposes a classification of semantic approaches into top-down and bottom-up, rely on the integration of external knowledge sources and a lightweight semantic representation based on the hypothesis [17]. To build a semantic recommendation system, researchers also proposed that adding a semantic matcher in recommendation system to match semantic documents from web service users [16]. Thus we plan to try similar recommendation system framework as to construct an online semantic content-based recommendation system. 

---

## Proposed Methods 

### Intuition 

- Comparing to other rental housing website like Zillow, we creatively come up with a new method to analyze data and present data people want to know. 
- The dataset we are using is comprehensive and timely. In terms of collecting data, we are using real time data because people want to know the up-to-date information. Besides, our dataset is comprehensive. For example, we analyze the crime data which is not included in all other websites’ datasets. 
- We also analyze data from users’ perspective. We rank the rental houses in three different ways, which are historical review ranking, convenience ranking and safety ranking. The data we show to users is useful, significant and innovative. These data are not user- accessible in other websites. 

### Data Collection and Refining

Our data includes downloaded data and scraped data. And there are 5 main sources of our data, including crime data downloaded from Atlanta Police Department, apartment data scraped from Zillow, reviews and rating data scraped from Yelp and via Google Map APIs, map data via Google Map API, and neighborhood convenience data scraped from Walkable. 

- For crime data: we downloaded 11 types crime data of Atlanta Police Department from 2009 to 2018. The data includes crime type, location, time and neighborhood. 

- For map data: based on the daily commute location, daily commute method and time limit, we calculate daily commute distance and then we get all the zipcodes in the circle with daily commute location as central and daily commute distance as radius. Further when we get candidate apartments, based on users’ interest places like gym or theater, we use Google Map API to get the closest users’ interest places around each candidate apartment and the distance between them. 

- For Zillow data, we used Python scraping to access apartment basic information by zipcodes we get in map data and users’ requirements from frontend. Then we remove all the apartments that don’t satisfy the time limit constraint. 

- For review and rating data, we use Python scraping to access each candidate apartment’s customer review and rating from Yelp and also get similar information via Google Map API. 

- For neighborhood convenience data, we use Python scraping to access information like walk score, bike score and transition score from Walkable by longitude and latitude information of each candidate apartment. 


![Screenshot](/assets/blogs/cse6242_report/figure1.jpg)
<center>Figure.1 Sample Crime Raw Dataset</center>

### Apartment Ranking 

In the beginning, users are required to select their daily commute location. Based on this location, 30 candidate apartments will be selected by scraping the data from Zillow. Then we would rank the apartments based on our algorithm. There are three criterions in ranking apartments, including historical review, safety and convenience. 

In order to rank the apartments, we calculate the scores for each apartment. For historical review scores, we directly scrape the apartment’s score on Yelp. For the convenience scores, the scores are obtained from Zillow by real-time scraping. The convenience score includes three parts which are walk score, bike score and transition score. 

For the safety score, we calculate the number of last 10 years’ accidents happened around each apartment. The equation of calculating the safety score is 

![Screenshot](/assets/blogs/cse6242_report/equation.jpg)

where Num i denotes the number of accidents happened around apartment i. Since we scrape 30 apartments from Zillow, the range of index i is from 1 to 30. Min denotes the minimum value among all Num i and Max denotes the maximum value among all Num i. 

Based on these three scores, the overall grade of each apartment is calculated by taking the average of these three scores. The recommendation ranking is done based on the overall grade. 

### User Interactive and Visualization

#### What Interface and their goals

- A welcome Interface: This page includes an introduction of our project and a simple tutorial about how to use it. It aims to help the users begin their first try on our
recommendation system.
- Search-filter Interface: This page has 4 sections. Basic information section contains
check-boxes, drop down menus and range sliders which enables users to customize their
preference including price range, beds, baths, rating on Google and Yelp, etc. In
transportation part, user need to locate their place of work/study and preferred distance or
time of commuting. In keyword search section, user can input their preferred places.
- Result Interface: A page shows a map with our recommended rental position ordered by
rank. In this page, users are able to scroll around the map and see on the map where our
recommended apartments are and the keyword search results. Other basic map operations
such as zoom in and out are also allowed.
- Detailed Information Interface: When users click a marker on in the result page, a
detailed information page will pop up, and tell them on why we recommend it.

#### How to build the interface (Tools)

- HTML5, CSS3 and JavaScipt: In general, our website can be divided into two main
functional parts. The first one is obtaining filter options from users and the second is
display our result in map. The fundamental tools we use are HTML5 with CSS3 and
Javascript. Meanwhile, some powerful library in Javascript like React are used to help us
build the website.
- Bootstrap (Search-filter Interface): After visiting the welcome page, a page with search
condition and user preference options module will be displayed. For our search filter
options, we consider Bootstrap a wonderful Javascript library to realize it. Bootstrap
contains various pre-built module for input section in map search, which is very efficient
and convenient if combined with CSS.
- For the actual display of map to show the recommendation result, we use Leafletjs library
which contains interactive component for map display such as marker and pop-up
function for showing details. We used the frame work in this Leafletjs library and
rendered our recommendations into the map.

### User Interactive and Visualization

#### What Interface and their goals
- A welcome Interface: This page includes an introduction of our project and a simple tutorial about how to use it. It aims to help the users begin their first try on our recommendation system.

- Search-filter Interface: This page has 4 sections. The Basic Information section contains checkboxes, drop down menus and range sliders which enables users to customize their preference on price range, number of beds and baths, pets policy, parking, etc. The Safety Requirement section contains two range sliders where users weigh their concern from 1 to 10 to violent crimes and property crimes. The Transportation Information section contains a search box where user can locate their place of work/ study (where they have to commute from home), a form where users input their prefered distance/ time of commuting and whether they walk or drive to commute. The Review Section enables users to select from 1 to 5 stars based on Yelp’s reviews thus serves to filter the recommendation results. 

- Result Interface: A page shows a map with our recommended rental position ordered by rank. In this page, users are able to scroll around the map and tap their interested positions to look for detail information and why we recommend it. Other basic map operations such as zoom in and out are also allowed.

#### How to build the interface (Tools)
- HTML5, CSS3 and JavaScipt: In general, our website can be divided into two main functional parts. The first one is obtaining filter options from users and the second is display our result in map. The fundamental tools we use are HTML5 with CSS3 and Javascript. Meanwhile, some powerful library in Javascript like React are used to help us build the website.

- React (Search-filter Interface):  After visiting the welcome page, a page with result map and user preference options module will be displayed. For our search filter options, we consider Reactivesearch a wonderful Javascript library to realize it. Reactivesearch contains various pre-built module for filter section in map search, which is very efficient and convenient. For the actual display of map, we use Reactivemap library which contains interactive component for map display.

#### Final Effect

![Screenshot](/assets/blogs/cse6242_report/welcome.jpg)
<center>Figure.2 Welcome Page</center>

![Screenshot](/assets/blogs/cse6242_report/search.jpg)
<center>Figure.3 Search Page</center>

![Screenshot](/assets/blogs/cse6242_report/result.jpg)
<center>Figure.4 Result Page</center>

---

## Evaluation

### Result and Experiment

For our recommendation system, we mainly want to add more features compared to existing
recommendation service to satisfy users’ safety anticipation and transportation requirement. Therefore, 
in order to evaluate our project, we asked for the feedback from our testing users. We first had the
testing users utilize our website for searching for their next potential rentals and then we gave them
the survey asking for their evaluation based on their experience with our website.

Also, since user experience is one of the top concern of a recommendation website, our website needs
to give back results as quick as possible to limit the user’s waiting time. Given that our website needs
to handle data from different sources and even with “real-time” scraping, efficiency of our codes are
essential. We want to have our website generates searching and ranking results within 2 seconds
based on our server’s computation resource. This time limit is our most important evaluation for
program efficiency and user experience.

Our questions had two categories. The first category was for asking experience with our website alone
and the other category was asking for comparison between our website and other existing solutions
such as zillow.com. We not only asked user’s for opinions on the results but also asked for user’s
satisfaction with our interface and visualization. By collecting the responses, we can answer the
questions for our project evaluations - how good is our recommendation results; how is our
recommendation system compared to others’; how is our website design.

### Detail of Evaluation

![Screenshot](/assets/blogs/cse6242_report/survey.jpg)
<center>Figure.5 Screenshot of User Survey</center>

We collected response from different groups of users for more accurate results. We had 40
total validate responses - 3 of them were freshmen or sophomores at Georgia Tech, 9 were
juniors or seniors at Georgia Tech, 15 were masters at Georgia Tech, 9 were PhD students at
Georgia Tech and 4 were daily commuters working around Georgia Tech campus area.
Below are the answers we got from our survey for testing users:

![Screenshot](/assets/blogs/cse6242_report/f6.jpg)
<center>Figure.6 Satisfaction for Recommendation</center>

![Screenshot](/assets/blogs/cse6242_report/f7.png)
<center>Figure.7 Satisfaction for Interface</center>

![Screenshot](/assets/blogs/cse6242_report/f8.jpg)
<center>Figure.8 Satisfaction Compared to Zillow</center>

For further quantifying how well our project’s recommendations were, we also asked users
whether or not they could found a satisfying result within the top 5 suggestions. Only 5 out of
40 users were unable to find one within the top 5 suggestions. The recommendation
algorithms and results yielded by the testing user survey met our expectation. However, our
website usually took more than 10 seconds for generating the result since we have faced
unforeseen challenge from scraping web pages. Our project failed meeting the 2-second
expected time limit. This was also the main reason why users deducted points from user
interface and visualization.



---

## Conclusion

In this project, we built a housing rental recommendation website, especially for off-campus
living students and daily commuters who have their special needs. There are three main
innovations for our project: First, this project innovatively comes up with the idea to design a
housing rental recommendation website especially for off-campus students and commuters.
Secondly, we used multiple datasets like Yelp, Zillow, crime dataset together to construct
multi-feature filters for our recommendation system. Finally, our user interface is interactive
and user-oriented. Our project will definitely contribute tremendous value in giving people an
easy life.

---

## Reference

[1] National Association of Realtors. Home Buyer and Seller Generational Trends Report 2017. National Association of Realtors, 2017.\\
[2] National Association of Realtors. Real Estate in a Digital Age Report 2017. National Association of Realtors, 2017.\\
[3] Fang, Yao-Min, et al. "An integrated information system for real estate agency-based on service-oriented architecture." Expert systems with applications 36.8 (2009): 11039-11044.\\
[4] Weisbrod, Glen, Steven R. Lerman, and Moshe Ben-Akiva. "Tradeoffs in residential location decisions: Transportation versus other factors." Transport Policy and Decision Making1.1 (1980): 13-26.\\
[5] Bina, Michelle, Valdemar Warburg, and Kara Kockelman. "Location choice vis-à-vis transportation: Apartment dwellers." Transportation Research Record: Journal of the Transportation Research Board 1977 (2006): 93-102.\\
[6] Mathur, Shishir. "Impact of transportation and other jurisdictional-level infrastructure and services on housing prices." Journal of Urban Planning and Development 134.1 (2008): 32-41.\\
[7] Frenkel, Amnon, Edward Bendit, and Sigal Kaplan. "Residential location choice of knowledge-workers: The role of amenities, workplace and lifestyle." Cities 35 (2013): 33-41.\\
[8] Kutsuzawa, Ryuji, et al. The impact of crime on housing land prices and the effects of police boxes and voluntary groups on crime prevention in Japan. No. 60. 2010.\\
[9] Zhang, Zhaohua, and Diane Hite. "House Value, Crime and Residential Location Choice." 2015 Annual Meeting, January 31-February 3, 2015, Atlanta, Georgia. No. 196826. Southern Agricultural Economics Association, 2015.\\
[10] Bayer, Patrick, Robert McMillan, and Kim Rueben. An Equilibrium Model of Sorting in an Urban Housing Market: The Causes and Consequences of Residential Segregation. No. 860. Center Discussion Paper, 2003.\\
[11] Tita, George E., Tricia L. Petras, and Robert T. Greenbaum. "Crime and residential choice: a neighborhood level analysis of the impact of crime on housing prices." Journal of quantitative criminology 22.4 (2006): 299.\\ 
[12] Lynch, Allen K., and David W. Rasmussen. "Measuring the impact of crime on house prices." Applied Economics 33.15 (2001): 1981-1989.\\
[13] Hellman, Daryl A., and Joel L. Naroff. "The impact of crime on urban residential property values." Urban Studies 16.1 (1979): 105-112.\\
[14] Gelman, Irit Askira, and Ningning Wu. "Combining structured and unstructured information sources for a study of data quality: a case study of Zillow. com." hicss. IEEE, 1899.\\
[15] Verma, Jai Prakash, Bankim Patel, and Atul Patel. "Big data analysis: recommendation system with Hadoop framework." Computational Intelligence & Communication Technology (CICT), 2015 IEEE International Conference on. IEEE, 2015.\\
[16] Manikrao, Umardand Shripad, and T. V. Prabhakar. "Dynamic selection of web services with recommendation system." Next Generation Web Services Practices, 2005. NWeSP 2005. International Conference on. IEEE, 2005.\\
[17] de Gemmis, Marco, et al. "Semantics-aware content-based recommender systems." Recommender Systems Handbook. Springer, Boston, MA, 2015. 119-159.\\
[18] Hurley, Gareth, and David C. Wilson. "DubLet: An Online CBR System for Rental Property Recommendation." International Conference on Case-Based Reasoning. Springer, Berlin, Heidelberg, 2001.\\
[19] Guy, Nicolas Nahimana. A Recommender system for rental properties. Diss. Strathmore University, 2017.\\
[20] Kasamani, Bernard Shibwabo, and Guy Nahimana. "A System for Recommending Rental Properties." Journal of Systems Integration 8.3 (2017): 10-18.\\





