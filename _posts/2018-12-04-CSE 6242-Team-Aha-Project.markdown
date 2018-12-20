---
title: "Rental Recommendation for Commuters"
layout: post
date: 2018-12-04 19:04
tag: 
    - CSE6242
    - Data Visualization  
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: A course project 
category: project
author: boshengjian
externalLink: false
---

## What's the Problem?

Nowadays, more than 90% people use web systems to buy or sell their real estates. But none of current systems is designed for meeting all special needs, such as safety concern and transportation requirement.

So we want to build a housing rental recommendation system for those who commutes daily, which can recommend housing to users their personal references, such as basic information, safety anticipation, and transportation requirement.

---

## What we have done

### Dataset Collection

**Crime Data**: Downloaded 11 types crime data of Atlanta Police Department from 2009 to 2018.
**Map Data**: Collected direction, distance, transportation data from Google map API.
**Zillow Data**: Used Python scraping to access and download each apartment’s basic information.
**Yelp Data**: Used Python scraping to apartments’information about reviews and rating.
**Walkable Data**: used Python scraping to access apartments’ convenience for transportation.

### Recommending
We access ranking based on users’ basic preference form Zillow, reviews and ratings from Yelp, amount of types of crime to qualify the safety anticipation of apartments, walkable score data for qualifying convenience of nearby transportation and direction information from Google Map API to fit users’ demand of specific places.

### Visualization
our website can be divided into two main functional parts. The first one is obtaining filter options from users and the second is display our result in map. The fundamental tools we use are HTML5 with CSS3 and JavaScript. Meanwhile, some powerful library like Leafletjs are used to help us build the website. Whenever we receive a search request from user, we will send a HTTP request to our back‐end server to get the recommendation result.

![Screenshot](/assets/projects/rent/map.png)
<center>The result page</center>

---

## Check it out:

See the code on Github: [https://github.com/bsjian/CSE6242-Team-Aha-Project](https://github.com/bsjian/CSE6242-Team-Aha-Project)

Check out the [demo](https://cse6242-224423.appspot.com/)

<iframe src="https://cse6242-224423.appspot.com/" width="100%" height="800">
  <p>Your browser does not support iframes.</p>
</iframe>

---

## To it locally:

(1)First, follow the README_BackEnd to run the backend server.

(2)Second, follow the README_FrontEnd to run the front end server.

**IMPORTANT: Please make sure that the local host and port number of backend server is correctly added to line 25 and line 27 in app.js in FrontEnd directory. (i.e. Postname:127.0.0.1.  Port: 3000)**

(3)Third, open your web browser and visit our website via the host and port number of your frontend program. (i.e. 127.0.0.1:3000)


## Back End

**NOTICE**

This project does contain scraping programs
that do not respect some websites' /robots.txt
We are aware that this is not a good manner and 
we are not using and will never use those code 
other than this class project. 

**IMPORTANT INFORMATION**

Execute the programs on large searching range or 
large number of searches in a short period of time 


There are 11 python files: 
- connectsql.py:              For querying the sqlite storing crime data;
- flaskhelper_revised.py:     Helper functions for main flask app;
- google_Distance.py:         For getting distance between locations using Google Map API; [fill in your API key at '# fill in your API key' before running]
- google_Score.py:            For getting place's rating on Google Map using Google Places API; [fill in your API key at '# fill in your API key' before running]
- google_Stores.py:           For getting nearby store information using Google Places API; [fill in your API key at '# fill in your API key' before running]
- price_as_feature.py:        For handling special price information format for apartments on Zillow;
- project_v2.py:              Main flask app; 
- scrape_zips.py:             For scraping zip codes within distance from a website; 
- yelp_scrape_real_time.py    For scraping real time yelp data;
- zillow_scrape_real_time.py  For scraping real time zillow data;
- crime_data_cleaning.py      For cleaning the crime dataset; 

There is also a folder 'crime_data' containing sqlite dataset storing crime data, original dataset and python file for cleaning the dataset. 

There is also a file 'test.json' containing a example/testing json file returned by backend. 

Main scraping python files have their own testing code for testing/debugging (IP banned issue or other cases). To get specific run instruction, execute:

    $ python yelp_scrape_real_time.py -help
    $ python zillow_scrape_real_time.py -help
    $ python price_as_feature.py -help

To run the main flask app 'project_v2.py' on server for public IP address visiting, please uncomment the line 215 and comment line 216. 

List for flask reroutings: 
- '/test':            return the testing test.json file; 
- '/':                return index.html for server debugging purpose only (note that the index.html file is not included, it can be any file ONLY for debugging purpose);
- '/apartmentprice'   return formatted prices for apartments on zillow;
- '/filters'          return list of found apartments satisfy the given filters;
- 

    [example: /filters?address='address'&type=h&beds=1&baths=1&pets=1&parking=1&laundry=1&price=500-1000&rate=3.5&review_count=10&places=xxx,yyy,zzz&travelling=driving&time_limit=20]

The backend files should be in below folder structure for successful running without modifying the code:

    /
    |__ connectsql.py
    |__ flaskhelper_revised.py
    |__ google_Distance.py
    |__ google_Score.py
    |__ price_as_feature.py
    |__ project_v2.py
    |__ scrape_zips.py
    |__ test.json
    |__ yelp_scrape_real_time.py
    |__ zillow_scrape_real_time.py
    |__ crime_data/
        |__ crime.db
        |__ crime_data_cleaning.py
        |__ crime_data.csv.zip
        |__ COBRA-2009-2017.csv.zip
        |__ cobra-2018.csv.zip


Before execute the backend, ensure you have installed following library in Python3: Flask, unicodecsv, requests, lxml, numpy

To execute the project's backend, first fill in your Google API key in line 5 of file google_Distance.py. Then execute: 
    
    $ sudo python project_v2.py

---


## Front End

### Live Demo

To see the app, go to [https://cse6242-224423.appspot.com/](https://cse6242-224423.appspot.com/)

### Features

* Responsive web design
* Customized Marker Icon in Leaflet.js map
 
### Getting Started

#### Clone or download this repository

```sh
git clone https://github.gatech.edu/bjian3/CSE6242-Front-End.git
```

#### Install dependencies

```sh
npm install
```

or

```sh
yarn install
```

#### Start Application

```sh
npm start
```

or

```sh
node app.js
```


### Built with

#### Front-end

* [ejs](http://ejs.co/)
* [Google Maps APIs](https://developers.google.com/maps/)
* [Bootstrap](https://getbootstrap.com/docs/3.3/)
* [Leaflet](https://leafletjs.com)
* [JQuery](https://jquery.com/)




