---
title: Design a Database Activity
permalink: /design/
classes: wide
---
In this activity, which you will work on during class time today and on Friday, you'll design the database for one of the major companies which use PostgreSQL today!

Your group will be assigned on of the following companites: Netflix, IMDb, Spotify, Twitch, Instagram, Reddit

With your group, develop 3 user personas for the company you have chosen; think about the following questions when developing your personas / researching the company:

- What does the end-user need from the database that this company uses to provide its service?  How will they interact with the company's database?
- What privileges will the end-user need to access the data?  Does this vary between end-users?
- What are the most common operations that the users will perform?
- Will your user have any knowledge of SQL?
- Will users be inserting / updating data, or just querying it?  Does this vary between users?

Once you have your user personas developed, start to write some ideas about the design of the database.  Include the following:

- What is the classification of the database you are developing?  (E.g., is it Single or multi-user?  General-purpose or discipline-specific?  So on and so forth...)
- What are the most common operations / transactions that you'd want to build into the system?
- What are the most common "roles" for users of the system?
- What types of data will be tracked by the system?  Will users be able to affect it?
- Are there actions taken by the system that are part of a larger transaction?  (E.g., if something can be purchased, what sequence of SQL statements would be executed?)

If you finish developing personas and discussing the database, begin to sketch a potential ERD for the database you've just discussed.
