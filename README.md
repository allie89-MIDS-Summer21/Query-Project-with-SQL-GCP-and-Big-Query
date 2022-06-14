# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback. 
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)
    Answer: There are 983648 total trips in this data.
    Query: 
    ```sql
    Select count (*) 
    From `bigquery-public-data`.san_francisco.bikeshare_trips;
    ```
- What is the earliest start date and time and latest end date and time for a trip?
    Answer: The earliest start date and time for a trip is 2013-08-29 09:08:00 UTC, whereas the latest is 2016-08-             31 23:48:00 UTC.
    Query:
    ```sql
    SELECT * FROM `bigquery-public-data`.san_francisco.bikeshare_trips
    ORDER BY start_date
    limit 5;
    SELECT * FROM `bigquery-public-data`.san_francisco.bikeshare_trips
    ORDER BY end_date desc 
    limit 5;```

- How many bikes are there?
    Answer: There are 700 unique bikes in the data.
    Query: 
    ```sql
    SELECT COUNT(distinct(bike_number))
    FROM `bigquery-public-data`.san_francisco.bikeshare_trips;```



### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: Which landmark has the most docks? 
  * Answer: San Francisco
    SQL query:
    ```sql
    SELECT count (dockcount), landmark FROM `bigquery-public-data.san_francisco.bikeshare_stations` 
    Group BY landmark
    Order by count(dockcount) desc;
    ```    
- Question 2: What is the average length of a trip?
  * Answer: 1019 sec, about 16 hours
  * SQL query:
  ```sql
   SELECT AVG(duration_sec) 
   FROM `bigquery-public-data.san_francisco.bikeshare_trips;` ```
    ```
- Question 3: Which starting stations have the most trips?
  * Answer: San Francisco Caltrain (Townsend at 4th)
  * SQL query:
  ```sql
   SELECT count(trip_ID), start_station_name 
   FROM `bigquery-public-data.san_francisco.bikeshare_trips
   Group BY start_station_name
   Order by count(trip_ID) desc;```
    
    
### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
   - Answer: There are 983648 total trips in this data.
   
   - Query: 
```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        From `bigquery-public-data.san_francisco.bikeshare_trips`'
    
```
 

  * What is the earliest start time and latest end time for a trip?
   - Answer: The earliest start date and time for a trip is 2013-08-29 09:08:00 UTC, whereas the latest is 2016-08-                  31 23:48:00 UTC.
   - Query: 
```
    bq query --use_legacy_sql=false '
    SELECT * FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    ORDER BY start_date     
    limit 5;'
  
    bq query --use_legacy_sql=false '
    SELECT * FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    ORDER BY end_date desc
    limit 5;'
   
```

 * How many bikes are there?
   - Answer: There are 700 unique bikes in the data.
   - Query: 
```
    bq query --use_legacy_sql=false '
    SELECT COUNT(distinct(bike_number)) FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    
```
        
2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    - Answer: I defined morning trips as any trip between the hours of 12am to 12pm and afternoon as anytime after               12pm. With that, I found there are 412339 trips in the morning and 571309 trips in the afternoon.
    - Query:
        ```sql
        select count(start_date),
             EXTRACT(HOUR from start_date) >= 0 AND EXTRACT(HOUR from start_date) < 12 as morning_trip,
            count(*) as trips,
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by morning_trip;
      ```
                     
        ```sql
        select count(start_date),
             EXTRACT(HOUR from start_date) >= 12 AND EXTRACT(HOUR from start_date) < 24 as afternoon_trip,
            count(*) as trips,
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by afternoon_trip;```

### Project Questions
- Identify the main questions you will need to answer to make recommendations (list below, add as many questions as you need).

- Question 1: Are there more trips taken by subscribers vs customers? 

- Question 2 Is there a difference in popular starting stations for customer vs a subscriber? If so, what is it?

- Question 3:Is there a difference of the average trip length for a customer vs subscriber? If so, what is it?

- Question 4: How many trips with starting and ending station are the same? Is there difference in customer vs subscriber?

- Question 5: How many trips with starting and ending station are different?  Is there difference in customer vs subscriber?

- Question 6: How many trips happen between rush hours? (7-9am or 4-7pm). These can be classified as commuter trips.

- Question 7: What is the average length of a commuter trip?

- Question 8: What are the most popular starting and ending stations for a commuter trip?

 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: Are there more trips taken by subscribers vs customers?

   -Answer: Yes. Trips by Customers: 136809 and Trips by Subscriber 846839

   -SQL query:
```sql
   SELECT count (distinct trip_ID), subscriber_type FROM `bigquery-public-data`.san_francisco.bikeshare_trips
   Group BY subscriber_type;
```

- Question 2: Is there a difference in popular starting stations for customer vs a subscriber? If so, what is it?
   
   -Answer:
        Top 3 for Customer:
                     1.Embarcadero at Sansome
                     2.Harry Bridges Plaza (Ferry Building)
                     3.Customer Market at 4th
        Top 3 for Subscriber:
                     1.San Francisco Caltrain (Townsend at 4th)
                     2.San Francisco Caltrain 2 (330 Townsend)
                     3.Temporary Transbay Terminal (Howard at Beale)               
   
   -SQL query:
```sql
    select subscriber_type, start_station_name, count(*) 
    from `bigquery-public-data`.san_francisco.bikeshare_trips
    group by start_station_name, subscriber_type
    having subscriber_type='Customer'
    order by count(start_station_name) desc; 
                     
    select subscriber_type, start_station_name, count(*) 
    from `bigquery-public-data`.san_francisco.bikeshare_trips
    group by start_station_name, subscriber_type
    having subscriber_type='Subscriber'
    order by count(start_station_name) desc; 
```
    
-  Question 3: Is there a difference of the average trip length for a customer vs subscriber? If so, what is it?
     
     -Answer:
          Customer: 62 minutes
          Subscriber: 10 minutes
     
     -SQL query:
```sql                 
        select  avg(duration_sec/60) , subscriber_type
        from `bigquery-public-data`.san_francisco.bikeshare_trips
        group by subscriber_type;
 ```
- Question 4: How many trips with starting and ending station are the same? Is there difference in customer vs subscriber?
   
    -Answer:
           Overall: 32047;          Customer: 22153, Subscriber 9894
   
   -SQL query:
```sql
        select count(trip_id)
        from `bigquery-public-data`.san_francisco.bikeshare_trips
        where start_station_name=end_station_name;
                     
        select count(trip_id), subscriber_type
        from `bigquery-public-data`.san_francisco.bikeshare_trips
        where start_station_name=end_station_name
        group by subscriber_type;
```
- Question 5: How many trips with starting and ending station are different?  Is there difference in customer vs      subscriber?
   
   -Answer: Overall: 951601; Customer: 114656. Subscriber:836945
   
   -SQL query:
```sql
      select count(trip_id)
      from `bigquery-public-data`.san_francisco.bikeshare_trips
      where start_station_name != end_station_name;
      
      select count(trip_id), subscriber_type
      from `bigquery-public-data`.san_francisco.bikeshare_trips
      where start_station_name != end_station_name
      group by subscriber_type;
```        
      
- Question 6: Which months have the most trips? Is this different for customers vs subscribers?
   
   -Answer: The top 3 most popular months for trips are August, October and June
              For customers, its: September, February, and August
              For subscribers, its: October, August, June
   
   -SQL query:
```sql
        select count(trip_id),
        EXTRACT(month from start_date) as trip_month,
        count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by trip_month
        order by count(trip_id) desc;
                                 
        select count(trip_id),
        EXTRACT(month from start_date) as trip_month,
        count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        where subscriber_type='Customer'
        group by trip_month
        order by count(trip_id) desc;

        select count(trip_id),
        EXTRACT(month from start_date) as trip_month,
        count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        where subscriber_type='Subscriber'
        group by trip_month
        order by count(trip_id) desc;
```       
                     
- Question 7: How many trips happen between rush hours? (7-9am or 4-7pm, on M-F). These can be classified as       commuter trips. 
    
    -Answer:
                     Rush Hour: 918028, Other Trips: 65620

   
   -SQL query:
```sql
        select
        (EXTRACT (Hour from start_date) >=7 AND extract(hour from end_date) <=9) OR Extract(hour from start_date) >= 16 or extract(hour from end_date)<=19 AND EXTRACT(DAYOFWEEK FROM start_date) IN (2, 3, 4, 5, 6) as rush_hour, 
        count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by rush_hour;                   
```
                     
- Question 8: What is the average length of a trip during rush hour?
   
   -Answer: Rush Hour trip: 15 minutes, Non-rush hour: 49 minutes
                     
   -SQL query:
```sql
      select avg(duration_sec/60),
             (EXTRACT (Hour from start_date) >=7 AND extract(hour from end_date) <=9) OR Extract(hour from cstart_date) >= 16 or extract(hour from end_date)<=19 AND EXTRACT(DAYOFWEEK FROM start_date) IN (2, 3, 4, 5, 6) as rush_hour, 
                count(*) as trips
      FROM `bigquery-public-data`.san_francisco.bikeshare_trips
      group by rush_hour;
```
                     
- Question 9: What are the most popular starting and ending stations for a commuter trip?
    
    -Answer:
      Top 3 Starting Stations:
                     1. San Francisco Caltrain (Townsend at 4th)
                     2. San Francisco Caltrain 2 (330 Townsend)
                     3. Harry Bridges Plaza (Ferry Building)
      Top 3 Ending Stations:
                   1. San Francisco Caltrain (Townsend at 4th)
                   2. San Francisco Caltrain 2 (330 Townsend)
                   3. Harry Bridges Plaza (Ferry Building)
   
   -SQL query:
```sql
                     
        select start_station_name,
            (EXTRACT (Hour from start_date) >=7 AND extract(hour from end_date) <=9) OR Extract(hour from start_date) >= 16 or extract(hour from end_date)<=19 as rush_hour, 
            count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by start_station_name,rush_hour
        order by count(start_station_name) desc;

        select end_station_name,
            (EXTRACT (Hour from start_date) >=7 AND extract(hour from end_date) <=9) OR Extract(hour from start_date) >= 16 or extract(hour from end_date)<=19 as rush_hour, 
            count(*) as trips
        FROM `bigquery-public-data`.san_francisco.bikeshare_trips
        group by end_station_name,rush_hour
        order by count(end_station_name) desc;
```
                                          
---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

