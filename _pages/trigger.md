---
title: Practice Accounting for User Needs
permalink: /trigger/
classes: wide
---

Download the airplanes schema from Sakai.  This schema came from a tutorial found online, which included the following logical ERD to document the schema: <https://postgrespro.com/docs/postgrespro/10/apjs02.html>.  The very first thing you'll want to do is sketch your own physical ERD for the format of the changes that have been made to attribute names and document the referential integrity of the schema.

Solve the following implementation challenges by using the documentation to design and develop relevant functions:

1. Allow travelers to use their booking number to select an available seat in the appropriate "class" on a flight for which they have a ticket but not a boarding pass.[^1]
2. Ensure that the traffic control operators do not accidentally schedule flights to land before they’ve taken off (i.e., correct any time-zone errors).
3. Ensure that travelers can get the details of their flight (departure time and timezone, destination airport and city, expected arrival time and timezone, seat, and flight status) by "logging in" to an application using their name and phone number. *They should not have to know to match the capitalization of their name or the format of their phone number!*
4. Provide a mechanism for data analysts to examine the frequency with which particular airplane models experience delays.
5. Protect your client’s data against ne'er-do-wells attempt SQL injection; focus specifically on locations where users will be required to populate text fields in forms (e.g., contact information) and ensure that they cannot damage your system or access data they shouldn’t have.[^2]


NOTE: You may solve these implementation challenges in any way you see fit!  You may find the following tips helpful as you consider options:

- Today's big goal is to figure out a plan to use the best tool for the given problem.  Make a plan, and determine how to execute that plan. **This is what you have to do on your final projects!!!**
- More than one of the challenges above require the use of a TRIGGER -- try multiple problems so that you can practice that new skill!
- There's hundreds of thousands of records; insert the data once and have a separate file for all of your testing / function development. *How will you test the things you're implementing?  What will you need to test?*
- This link takes you to strategies for manipulating and comparing date/time data values: <https://www.postgresql.org/docs/current/functions-datetime.html>
- This link takes you to a whole bunch of functions which can manipulate text values: <https://www.postgresql.org/docs/14/functions-string.html>
- Variables can be initialized from the results of a query (provided the query returns only a single value).
- Don't forget that you can trigger on data changes to a VIEW!

[^1]: Assume an interface will have a button they can press to choose their seat from a list of options, which removes that record as an option for other travelers on the same flight.  By deleting a row from their list, a boarding pass will be issued for the traveler to use that seat.

[^2]: Consider this link as guidance for what could be attempted: <https://www.onsecurity.io/blog/pentesting-postgresql-with-sql-injections/>
