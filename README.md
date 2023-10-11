# London Bike-sharing Tips
You can download the dataset from my Github repository: https://github.com/rodrickmascarenhas/london-bikes/

The London bicycle hires dataset is a collection of information about the usage of the Santander Cycle Hire Scheme in London, which is a public bicycle sharing system that allows people to rent bikes from different locations across the city.

Here the data includes the number of hires, the start and end dates and stations, the bike IDs, and the trip duration in minutes. The data can be used for various purposes, such as analyzing the patterns and trends of bike sharing, identifying the most popular routes and stations, and evaluating the impact of bike sharing on transport and environment.

## Data preparation instructions

Create new project in Google Cloud Platform > Set up Billing

image

IAM > Find account associated with your project > Add the BigQuery Data Viewer permissions with "Owner"

image

Search for "bigquery-public-data" datasets and click the starred icon

image

Expand the view and select london_bikes > (cycle_hire, cycle_stations) > Copy to a dataset in your project

```sql
with h as (SELECT * FROM `bigquery-ids.london_bikes.cycle_hire`),
ss as (SELECT * FROM `bigquery-ids.london_bikes.cycle_stations`),
es as (SELECT * FROM `bigquery-ids.london_bikes.cycle_stations`)
select rental_id, duration, bike_id, bike_model,
start_station_id, start_station_name, start_date, end_station_id, end_station_name, end_date,
ss.installed start_installed, es.installed end_installed,
ss.latitude start_latitude, ss.longitude start_longitude,
es.latitude end_latitude, es.longitude end_longitude
from h
left join ss on start_station_id = ss.Id
left join es on end_station_id = es.Id limit 10000;
```

## Data cleansing

Go to DataPrep > Design a workflow ingesting the 'cycle_hire' dataset > Edit Recipe > Data Transformation

image

Select the original dataset as the destination (Note: Ensure the region in dataset is same as original dataset) > Run the flow > Completed

Perform the following SQL query to join the tables > Save as 'query.sql'

image

Go to Tableau > Data > Add Data Source > Google BigQuery > Sign In using OAuth

Add BillingProject > Add Project > Add Dataset

## Exploratory Data Analysis



## Results

<ul>
<li>2016 was the year with the most ride time and the the longest aver trip duration.</li>
<li>The most trips began and ended at the same five stations.</li>
<li>The top five bike with the most trips made 15-17 trips that day.</li>
<li>The most trips began between the hours of 4 pm and 5 pm (2800 in that hour alone).</li>
</ul>

On average,
<ul>
<li>The most trips start from Belgrove Street , King's Cross station.</li>
<li>The longest trips start from Black Lion Gate, Kensington Gardens station.</li>
<li>The least trips start from Here East South, Queen Elizabeth Olympic Park station.</li>
<li>We see that five stations have the shortest trip duration.</li>
<li>The number of trips from these stations are also above average so these stations may be in a high traffic area where people are commuting short distances frequently.</li>
</ul>
