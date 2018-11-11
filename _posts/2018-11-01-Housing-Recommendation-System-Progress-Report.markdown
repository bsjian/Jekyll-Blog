---
title: "Housing Recommendation System - Progress Report"
layout: post
date: 2018-11-01 23:18
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Data Visualization
- CSE6242
category: blog
author: boshengjian
description: CSE6242 Progress Report 
---

## Summary

See the PDF version [here](https://drive.google.com/file/d/1viFWu8ctGKZJ1f2qpPUF07AvfS8P--Jf/view?usp=sharing
).

This is the progress report for CSE6242 Data and Visual Analytics.

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
 
Those factors are of concern when people search for housing : location, type, rent price, number of bedrooms, number of bathrooms, the area of living space, availability of parking space, whether they are furnished or serviced and the daily commuting distance and time. Many researches have been done to analyze the relationship between transportation and other metrics based on renters’ choices of renting [4,5,6,7]. Researches also show that the renters’ choices are also highly related with their occupations. Those researches show how we may improve our project in the future, which is using the date came from our own website to build model and improve the recommendation system.

At the same time, crime rate is also important in residential decision. Researchers found out that people are willing to pay more to move to a location with lower violent crime occurrences [8,10]. Also, crime rate and house price are innately correlated. George et al used instrumental variable (IV) method and concluded that every 10% decrease in burglaries rate would result an average 1% rise in land price in Japan [11]. As for crime types, researchers have differentiated property crime and violent crime, and quantify the ‘‘intangible cost’’  by using hedonic regression [12,13,14]. The results suggests that violent crime is the dominant factor affecting house prices. Thus we aim to only focus on violent crimes in Atlanta.

Our system uses multiple datasets from different sources, including user input data and verified or unverified public datasets. Gelman et al points out that plenty of information input by end-users are inaccurate [14]. The author uses data event analysis and error cluster analysis for investigating errors. Data event analysis involves a review of all the processes that capture or manipulate the data and error cluster analysis centers on a data subset that contains data suspected of being incorrect. We plan to use similar methods to verify inaccurate data and clean inaccurate data
.
As for recommendation system, Verma et al built a recommendation system with Hadoop and utilizes recommendation algorithms in Mahout Architecture [15]. To solve natural language ambiguity, Manikrao et al proposed a classification of semantic approaches into top-down and bottom-up, rely on the integration of external knowledge sources and a lightweight semantic representation based on the hypothesis [17]. To build an semantic recommendation system, we can add a semantic matcher to match semantic documents from web service users [16]. Thus we plan to try similar recommendation system framework as to construct an online semantic content-based recommendation system.

---

## Proposed Methods 

### Data Collection and Refining

#### Data Collection 

There are several datasets we are using including Yelp dataset, Zillow dataset, as well as Atlanta crime dataset. 

For Yelp dataset, we use Python scraping to access and download each apartment’s basic information, like the location, zip code, customer review, ranking, score and so on. Since continuously craping Yelp can result being banned by IP, we decide to “scrape as we need” like doing search automatically for users. 

For Zillow dataset, we are not able to use scraping to access the dataset, since Zillow has its anti-crawl system. Hence, Zillow api are applying to retrieve data from Zillow data. The main information include location, ranking, price, house type, public infrastructure, entertainments, pet, parking lot, washer as well as the ranking.

For Atlanta crime dataset, the dataset will include all the up-to-date information about crimes happened from 2009-2018. The dataset include crime type, location, time and neighborhood. 

In order to combine the information about locations in all datasets, we are going to use Google Map API to retrieve the latitude and longitude. 
The sample raw dataset are shown below, which should be cleaned and collected afterwords. 

![Screenshot](/assets/images/blogs/cse6242_progress/figure1.jpg)
<center>Figure.1 Sample Crime Raw Dataset</center>

#### Data Refining

We use Zillow’s dataset to get apartments’ basic information, at the same time we use the scores in Yelp as an additional feature, but for some specific apartments in Zillow we can’t find corresponding scores in Yelp. Thus we propose two ways to solve this data missing problem.

- Average Interpolation:
For those score missing apartments, we use the average scores in the whole     Yelp dataset as the value of missing scores.

- Logistic Regression Model Prediction:
Based on Yelp dataset and corresponding information in Zillow dataset, we train a Logistic Regression model. Then based on this trained model, we use the information of score missing apartments in Zillow dataset as inputs and get the predicted scores.

- Regression Tree Model Prediction:
Based on Yelp dataset and corresponding information in Zillow dataset, we train a Regression Tree model. Then based on this trained model, we use the information of score missing apartments in Zillow dataset as inputs and get the predicted scores.

### Multi-feature Filtering
Based on basic information that user choose we use Zillow’s dataset to filter 30 candidate apartments, the first has initial score of 30 and the last has initial score of 1. 

For feature transportation, crime rates and scores from Yelp, we set corresponding weights.

Compute the final score of each candidate apartments based on initial scores and additional features scores . 

Based on this final scores, show the top 10 apartments with basic information on the website for user to choose.

### User Interactive and Visualization

#### What Interface and their goals
- A welcome Interface: This page includes an introduction of our project and a simple tutorial about how to use it. It aims to help the users begin their first try on our recommendation system.

- Search-filter Interface: This page has 4 sections. The Basic Information section contains checkboxes, drop down menus and range sliders which enables users to customize their preference on price range, number of beds and baths, pets policy, parking, etc. The Safety Requirement section contains two range sliders where users weigh their concern from 1 to 10 to violent crimes and property crimes. The Transportation Information section contains a search box where user can locate their place of work/ study (where they have to commute from home), a form where users input their prefered distance/ time of commuting and whether they walk or drive to commute. The Review Section enables users to select from 1 to 5 stars based on Yelp’s reviews thus serves to filter the recommendation results. 

- Result Interface: A page shows a map with our recommended rental position ordered by rank. In this page, users are able to scroll around the map and tap their interested positions to look for detail information and why we recommend it. Other basic map operations such as zoom in and out are also allowed.

#### How to build the interface (Tools)
- HTML5, CSS3 and JavaScipt: In general, our website can be divided into two main functional parts. The first one is obtaining filter options from users and the second is display our result in map. The fundamental tools we use are HTML5 with CSS3 and Javascript. Meanwhile, some powerful library in Javascript like React are used to help us build the website.

- React (Search-filter Interface):  After visiting the welcome page, a page with result map and user preference options module will be displayed. For our search filter options, we consider Reactivesearch a wonderful Javascript library to realize it. Reactivesearch contains various pre-built module for filter section in map search, which is very efficient and convenient. For the actual display of map, we use Reactivemap library which contains interactive component for map display.

#### Final Effect

![Screenshot](/assets/images/blogs/cse6242_progress/welcome.jpg)
<center>Figure.2 Welcome Page</center>

![Screenshot](/assets/images/blogs/cse6242_progress/search.jpg)
<center>Figure.3 Search Page</center>

---

## Evaluation

For our recommendation system, we mainly want to add more features compared to existing recommendation service to satisfy users’ safety anticipation and transportation requirement. Therefore, our primary evaluation criteria are users’ feedback and user satisfactory. Designed survey would be sent out to our test users and based on these feedback, a user oriented evaluation can be made to our recommendation website. The survey will be a blind test. The user will not know if the recommandation is from our system or other existing solutions so that we can compare the feedback to see whether or not our recommendation is better than others. 

The survey will also include the feedback on our website design - appearance, usability, cleanness and so on - as a evaluation of our user-end design. 

Also, since user experience is one of the top concern of a recommendation website, our website needs to give back results as quick as possible to limit the user’s waiting time. Given that our website needs to handle data from different sources and even with “real-time” scraping, efficiency of our codes are essential. We want to have our website generates searching and ranking results within 2 seconds based on our server’s computation resource. This time limit will be our most important evaluation for program efficiency and user experience. 

---

## Conclusion

In this project, we are going to build  a housing rental recommendation website, especially for off-campus living students and daily commuters who have their special needs. This design will definitely contribute tremendous value in giving people a easy life. The project’s  main innovation are listed below.

- This project innovatively comes up with the idea to design a housing rental recommendation website especially for off-campus students and commuters.

- Multiple datasets like Yelp, Zillow, Crime datasets are used to construct multi-feature filtings.

- The user interactive interface and visualization are going to be built.

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





