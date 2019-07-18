---
layout: post
title: Visualize Twitter Network using Python
description: >
  Using python and gephi to visualize a twitter network
tags: [visuals, python]
---

Visualize a social network is a very common graph analysis. Here's an
implementation for a Twitter network of Elon Musk using tweepy and gephi.

First we will utilize the Twitter API. Once you get the key and token codes,
we can start requesting data through the keys.

~~~python
auth = tweepy.OAuthHandler(api_key, api_secret)
auth.set_access_token(token, token_secret)
api = tweepy.API(auth)
~~~

Next we can get the top 20 friends using tweepy. Twitter API has a limit where
one can only send a number of request per mins, so a sleep timer can be used to
get around that:

~~~python
friendList = [];
while True:
try:
    for user in tweepy.Cursor(api.friends, screen_name="elonmusk").items(20):
        friendList.append((root_user, user.screen_name))
    break;

    except tweepy.RateLimitError:
        time.sleep(60);
        continue;
~~~

We can also get followers for each friend from the friend list that we just got.

~~~python
followerList = [];
for friend_tuple in friendList:
while True:
    try:
        for user in tweepy.Cursor(api.followers, friend_tuple[1]).items(20):
            followerList.append((friend_tuple[1], user.screen_name))
        break;

    except tweepy.RateLimitError:
        time.sleep(60);
        continue;
~~~

Putting the 2 arrays together:

~~~python
all = friendList + followerList;
~~~

And we can write the data into a csv file, which should just have 2 columns.
The first column should be the source and the second column should be the friends
or follower of the source. The table should look like this:

 |elonmusk|TalulahRiley|
 |elonmusk|imgur|
 |elonmusk|RickandMorty|

Now we can load the table into gephi. The settings can be modified. The below
plot is made with "force atlas" layout. Node sizes and node label sizes are
adjusted by proportional weight.

<img src="{% asset 'elon.png' @path %}" alt="elon" />

The settings of gephi can be customized in detail. For tutorials on how to use
gephi, here's a link to documentations that helped a lot:

[Gephi Tutorials](https://seinecle.github.io/gephi-tutorials/)
