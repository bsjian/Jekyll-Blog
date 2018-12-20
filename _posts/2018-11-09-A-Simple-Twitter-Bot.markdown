---
title: "A Simple Twitter Bot"
layout: post
date: 2018-11-09 23:03
tag: 
    - Python  
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: A weather forecast Chrome Extension
category: project
author: boshengjian
externalLink: false
---

I just created a very simple twitter bot based on Tweepy. 

When you @BBosheng with hash tag #helloworld, the bot will reply you immediately!


{% twitter https://twitter.com/jianbosheng/status/1069705784127299590 %}

{% twitter https://twitter.com/BBosheng/status/1069706170519183360 %}

---

## Twitter Developer Account

Go to [developer.twitter.com](https://developer.twitter.com/) and apply for a developer account.

Click apps and create a new App.

![Screenshot](/assets/projects/twitter-bot/click-app.png)

Get API Keys:

![Screenshot](/assets/projects/twitter-bot/api.png)

I also used [Tweepy](http://www.tweepy.org/) to access the Twitter API. Check out the docs [here](https://tweepy.readthedocs.io/en/v3.5.0/)

---

## The Logic

1. Get the Tweet Timelines which the Bot is mentioned. 
2. The Bot replies all the mentioning.
3. The Bot keeps track of the last tweet he replied to make it faster.
 
### Get Tweet Timelines

#### Example Request
    GET https://api.twitter.com/1.1/statuses/mentions_timeline.json?count=2&since_id=14927799

#### Example Response

```Javascript
[
  {
    "coordinates": null,
    "favorited": false,
    "truncated": false,
    "created_at": "Mon Sep 03 13:24:14 +0000 2012",
    "id_str": "242613977966850048",
    "entities": {
      "urls": [

      ],
      "hashtags": [

      ],
      "user_mentions": [
        {
          "name": "Jason Costa",
          "id_str": "14927800",
          "id": 14927800,
          "indices": [
            0,
            11
          ],
          "screen_name": "jasoncosta"
        },
        {
          "name": "Matt Harris",
          "id_str": "777925",
          "id": 777925,
          "indices": [
            12,
            26
          ],
          "screen_name": "themattharris"
        },
        {
          "name": "ThinkWall",
          "id_str": "117426578",
          "id": 117426578,
          "indices": [
            109,
            119
          ],
          "screen_name": "thinkwall"
        }
      ]
    },
    "in_reply_to_user_id_str": "14927800",
    "contributors": null,
    "text": "@jasoncosta @themattharris Hey! Going to be in Frisco in October. Was hoping to have a meeting to talk about @thinkwall if you're around?",
    "retweet_count": 0,
    "in_reply_to_status_id_str": null,
    "id": 242613977966850048,
    "geo": null,
    "retweeted": false,
    "in_reply_to_user_id": 14927800,
    "place": null,
    "user": {
      "profile_sidebar_fill_color": "EEEEEE",
      "profile_sidebar_border_color": "000000",
      "profile_background_tile": false,
      "name": "Andrew Spode Miller",
      "profile_image_url": "http://a0.twimg.com/profile_images/1227466231/spode-balloon-medium_normal.jpg",
      "created_at": "Mon Sep 22 13:12:01 +0000 2008",
      "location": "London via Gravesend",
      "follow_request_sent": false,
      "profile_link_color": "F31B52",
      "is_translator": false,
      "id_str": "16402947",
      "entities": {
        "url": {
          "urls": [
            {
              "expanded_url": null,
              "url": "http://www.linkedin.com/in/spode",
              "indices": [
                0,
                32
              ]
            }
          ]
        },
        "description": {
          "urls": [

          ]
        }
      },
      "default_profile": false,
      "contributors_enabled": false,
      "favourites_count": 16,
      "url": "http://www.linkedin.com/in/spode",
      "profile_image_url_https": "https://si0.twimg.com/profile_images/1227466231/spode-balloon-medium_normal.jpg",
      "utc_offset": 0,
      "id": 16402947,
      "profile_use_background_image": false,
      "listed_count": 129,
      "profile_text_color": "262626",
      "lang": "en",
      "followers_count": 2013,
      "protected": false,
      "notifications": null,
      "profile_background_image_url_https": "https://si0.twimg.com/profile_background_images/16420220/twitter-background-final.png",
      "profile_background_color": "FFFFFF",
      "verified": false,
      "geo_enabled": true,
      "time_zone": "London",
      "description": "Co-Founder/Dev (PHP/jQuery) @justFDI. Run @thinkbikes and @thinkwall for events. Ex tech journo, helps run @uktjpr. Passion for Linux and customises everything.",
      "default_profile_image": false,
      "profile_background_image_url": "http://a0.twimg.com/profile_background_images/16420220/twitter-background-final.png",
      "statuses_count": 11550,
      "friends_count": 770,
      "following": null,
      "show_all_inline_media": true,
      "screen_name": "spode"
    },
    "in_reply_to_screen_name": "jasoncosta",
    "source": "JournoTwit",
    "in_reply_to_status_id": null
  },
  {
    "coordinates": {
      "coordinates": [
        121.0132101,
        14.5191613
      ],
      "type": "Point"
    },
    "favorited": false,
    "truncated": false,
    "created_at": "Mon Sep 03 08:08:02 +0000 2012",
    "id_str": "242534402280783873",
    "entities": {
      "urls": [

      ],
      "hashtags": [
        {
          "text": "twitter",
          "indices": [
            49,
            57
          ]
        }
      ],
      "user_mentions": [
        {
          "name": "Jason Costa",
          "id_str": "14927800",
          "id": 14927800,
          "indices": [
            14,
            25
          ],
          "screen_name": "jasoncosta"
        }
      ]
    },
    "in_reply_to_user_id_str": null,
    "contributors": null,
    "text": "Got the shirt @jasoncosta thanks man! Loving the #twitter bird on the shirt :-)",
    "retweet_count": 0,
    "in_reply_to_status_id_str": null,
    "id": 242534402280783873,
    "geo": {
      "coordinates": [
        14.5191613,
        121.0132101
      ],
      "type": "Point"
    },
    "retweeted": false,
    "in_reply_to_user_id": null,
    "place": null,
    "user": {
      "profile_sidebar_fill_color": "EFEFEF",
      "profile_sidebar_border_color": "EEEEEE",
      "profile_background_tile": true,
      "name": "Mikey",
      "profile_image_url": "http://a0.twimg.com/profile_images/1305509670/chatMikeTwitter_normal.png",
      "created_at": "Fri Jun 20 15:57:08 +0000 2008",
      "location": "Singapore",
      "follow_request_sent": false,
      "profile_link_color": "009999",
      "is_translator": false,
      "id_str": "15181205",
      "entities": {
        "url": {
          "urls": [
            {
              "expanded_url": null,
              "url": "http://about.me/michaelangelo",
              "indices": [
                0,
                29
              ]
            }
          ]
        },
        "description": {
          "urls": [

          ]
        }
      },
      "default_profile": false,
      "contributors_enabled": false,
      "favourites_count": 11,
      "url": "http://about.me/michaelangelo",
      "profile_image_url_https": "https://si0.twimg.com/profile_images/1305509670/chatMikeTwitter_normal.png",
      "utc_offset": 28800,
      "id": 15181205,
      "profile_use_background_image": true,
      "listed_count": 61,
      "profile_text_color": "333333",
      "lang": "en",
      "followers_count": 577,
      "protected": false,
      "notifications": null,
      "profile_background_image_url_https": "https://si0.twimg.com/images/themes/theme14/bg.gif",
      "profile_background_color": "131516",
      "verified": false,
      "geo_enabled": true,
      "time_zone": "Hong Kong",
      "description": "Android Applications Developer,  Studying Martial Arts, Plays MTG, Food and movie junkie",
      "default_profile_image": false,
      "profile_background_image_url": "http://a0.twimg.com/images/themes/theme14/bg.gif",
      "statuses_count": 11327,
      "friends_count": 138,
      "following": null,
      "show_all_inline_media": true,
      "screen_name": "mikedroid"
    },
    "in_reply_to_screen_name": null,
    "source": "Twitter for Android",
    "in_reply_to_status_id": null
  }
]
```

### Reply

I used Tweepy's update_status function to automatically reply to someone once was mentioned. 

{% highlight html %}
api.update_status('@' + mention.user.screen_name + '#HelloWorld back to you!', mention.id)
{% endhighlight %}

### Keep Track of the Last Replied Mention

By creating a file to store the Mention id we can easily manipulate the last seen mention. 

When getting the Tweet Timeline, we no longer need to get tweets beyond the last seen tweet. When replying, marking the current processing Tweet as the last seen tweet (thus write its id into the file).

---

There are tons of tutorials online, check this out:



<iframe width="560" height="310" src="https://www.youtube.com/embed/W0wWwglE1Vc" frameborder="0" allowfullscreen></iframe>

