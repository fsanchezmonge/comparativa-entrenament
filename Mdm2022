# -- Montseny22 --

import pandas as pd
from datetime import datetime, timedelta, date
import matplotlib.pyplot as plt
import numpy as np

#path to your excel file
file_path = '/data/notebook_files/mdm2022'

#use the `read_excel()` function to import the file as a DataFrame
montseny_df = pd.read_csv(file_path)

# Convert the date column to a Pandas datetime object
montseny_df['date'] = pd.to_datetime(montseny_df['date'])

# Format the date column to mm/dd/yyyy format
montseny_df['date'] = montseny_df['date'].dt.strftime('%m/%d/%Y')
#print(df)

# Convert the date column to datetime format
montseny_df['date'] = pd.to_datetime(montseny_df['date'])

start_date = pd.to_datetime('2022-09-01')
end_date = pd.to_datetime('2022-11-12')
montseny_df = montseny_df[(montseny_df['date'] >= start_date) & (montseny_df['date'] <= end_date)]

#print(montseny_df)

stats = montseny_df.describe()
print(stats)

import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter

df_moving_time = montseny_df.groupby(['date'])['moving_time'].agg('sum').reset_index()
df_moving_time['date'] = pd.to_datetime(df_moving_time['date'])

start_date = df_moving_time['date'].min()
end_date = df_moving_time['date'].max()
date_range = pd.date_range(start=start_date, end=end_date, freq='D')

# create a new dataframe with all dates in the range
daily_df = pd.DataFrame({'date': date_range})
daily_df['date'] = pd.to_datetime(daily_df['date'])
#print(daily_df)
# Merge the two data frames on the "date" column
df_moving_time_daily = pd.merge(df_moving_time, daily_df, on='date', how='right')
df_moving_time_daily['moving_time'] = df_moving_time_daily['moving_time'].fillna(0)

# Optionally, sort the data frame by date
df_moving_time_daily = df_moving_time_daily.sort_values('date')

# Convert the date column to datetime type (if it's not already)
df_moving_time_daily['date'] = pd.to_datetime(df_moving_time_daily['date'])

# Set the date column as the index of the data frame
df_moving_time_daily.set_index('date', inplace=True)

# Calculate the rolling 7-day sum of training duration
df_moving_time_daily['rolling_duration'] = df_moving_time_daily['moving_time'].rolling('7D').sum()

# Reset the index of the data frame (if needed)
df_moving_time_daily.reset_index(inplace=True)

plt.figure(figsize=(10, 6))  # Set the desired width and height of the figure
plt.bar(df_moving_time_daily.index, df_moving_time_daily['rolling_duration'],color='lightblue')
date_formatter = DateFormatter('%m-%d')
plt.gca().xaxis.set_major_formatter(date_formatter)

# Set the x-axis label
plt.xlabel('Date')

# Set the y-axis label
plt.ylabel('Rolling 7-day duration')
plt.title('Últims 2 mesos pre-Montseny')

# Display the plot
plt.show()

average_rolling = round(df_moving_time_daily['rolling_duration'].mean())
std_rolling = round(df_moving_time_daily['rolling_duration'].std())

average_moving = round(df_moving_time_daily['moving_time'].mean())

print(f"Average rolling 7-day duration is: {average_rolling} minutes")
print(f"Standard deviation for rolling duration is: {std_rolling} minutes")
print(f"Average moving time is: {average_moving} minutes")

import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter

df_elevation = montseny_df.groupby(['date'])['elevation_gain'].agg('sum').reset_index()
df_elevation['date'] = pd.to_datetime(df_elevation['date'])

start_date = df_elevation['date'].min()
end_date = df_elevation['date'].max()
date_range = pd.date_range(start=start_date, end=end_date, freq='D')

# create a new dataframe with all dates in the range
daily_df = pd.DataFrame({'date': date_range})
daily_df['date'] = pd.to_datetime(daily_df['date'])
#print(daily_df)
# Merge the two data frames on the "date" column
df_elevation_daily = pd.merge(df_elevation, daily_df, on='date', how='right')
df_elevation_daily['elevation_gain'] = df_elevation_daily['elevation_gain'].fillna(0)

# Optionally, sort the data frame by date
df_elevation_daily = df_elevation_daily.sort_values('date')

# Convert the date column to datetime type (if it's not already)
df_elevation_daily['date'] = pd.to_datetime(df_elevation_daily['date'])

# Set the date column as the index of the data frame
df_elevation_daily.set_index('date', inplace=True)

# Calculate the rolling 7-day sum of training duration
df_elevation_daily['rolling_elevation_gain'] = df_elevation_daily['elevation_gain'].rolling('7D').sum()

# Reset the index of the data frame (if needed)
df_elevation_daily.reset_index(inplace=True)

plt.figure(figsize=(10, 6))  # Set the desired width and height of the figure
plt.bar(df_elevation_daily.index, df_elevation_daily['rolling_elevation_gain'], color='lightblue')
date_formatter = DateFormatter('%m-%d')
plt.gca().xaxis.set_major_formatter(date_formatter)

# Set the x-axis label
plt.xlabel('Date')

# Set the y-axis label
plt.ylabel('Rolling 7-day elevation gain')
plt.title('Últims 2 mesos pre-Montseny')

# Display the plot
plt.show()

average_rolling_elev = round(df_elevation_daily['rolling_elevation_gain'].mean())
std_rolling_elev = round(df_elevation_daily['rolling_elevation_gain'].std())

average_elev = round(df_elevation_daily['elevation_gain'].mean())

print(f"Average rolling 7-day elevation gain is: {average_rolling_elev} meters")
print(f"Standard deviation for rolling elevation gain is: {std_rolling_elev} meters")
print(f"Average elevation gain is: {average_elev} meters")


# Sort the DataFrame by date in ascending order
montseny_df.sort_values('date', inplace=True)

# Calculate the rest days between consecutive training sessions
montseny_df['rest_days'] = montseny_df['date'].diff().dt.days - 1

# Calculate the average rest days
average_rest_days = montseny_df['rest_days'].mean()

print("Average rest days between trainings:", round(average_rest_days,3))

import matplotlib.pyplot as plt

# Assuming your DataFrame is named 'df' and the distance column is named 'distance'

# Create distance intervals of 5km from 0 to 50
bins = range(0, 55, 5)

# Assign each run to a distance interval
montseny_df['distance_interval'] = pd.cut(montseny_df['distance'], bins=bins, right=False)

# Count the number of runs in each distance interval
runs_per_interval = montseny_df['distance_interval'].value_counts().sort_index()

# Plot the bar graph
plt.figure(figsize=(8, 5))  # Set the desired width and height of the figure
plt.bar(runs_per_interval.index.astype(str), runs_per_interval.values)

# Add labels and title
plt.xlabel('Distance Interval (km)')
plt.ylabel('Frequency')
plt.title('Number of Runs vs. Distance Intervals')

# Rotate x-axis labels if needed
plt.xticks(rotation=40)

# Display the graph
plt.show()

import matplotlib.pyplot as plt

# Assuming your DataFrame is named 'df' and the distance column is named 'distance',
# and the elevation gain column is named 'elevation_gain'

# Plot each run as a point
plt.scatter(montseny_df['distance'], montseny_df['elevation_gain'])

# Add labels and title
plt.xlabel('Distance (km)')
plt.ylabel('Elevation Gain (m)')
plt.title('Elevation Gain vs. Distance')

# Display the graph
plt.show()

import matplotlib.pyplot as plt

# Assuming your DataFrame is named 'df' and the distance column is named 'distance'

# Create distance intervals of 5km from 0 to 50
bins = range(0, 300, 60)

# Assign each run to a distance interval
montseny_df['duration_interval'] = pd.cut(montseny_df['moving_time'], bins=bins, right=False)

# Count the number of runs in each distance interval
runs_per_interval = montseny_df['duration_interval'].value_counts().sort_index()

# Plot the bar graph
plt.figure(figsize=(8, 5))  # Set the desired width and height of the figure
plt.bar(runs_per_interval.index.astype(str), runs_per_interval.values)

# Add labels and title
plt.xlabel('Duration Interval (minutes)')
plt.ylabel('Frequency')
plt.title('Number of Runs vs. Duration Intervals Montseny')

# Rotate x-axis labels if needed
plt.xticks(rotation=40)

# Display the graph
plt.show()


# Create a new column for the week number
montseny_df['week_number'] = montseny_df['date'].dt.week

# Group the DataFrame by week number and count the number of training sessions
trainings_per_week = montseny_df.groupby('week_number').size()

print("Sessions per week:")
print(trainings_per_week)
print("Average sessions per week")
print(round(trainings_per_week.mean(),2))


import matplotlib.pyplot as plt

# Assuming your DataFrame is named 'df' and the distance column is named 'distance'

# Create distance intervals of 5km from 0 to 50
bins = range(0, 3000, 500)

# Assign each run to a distance interval
montseny_df['elevation_interval'] = pd.cut(montseny_df['elevation_gain'], bins=bins, right=False)

# Count the number of runs in each distance interval
runs_per_interval = montseny_df['elevation_interval'].value_counts().sort_index()

# Plot the bar graph
plt.bar(runs_per_interval.index.astype(str), runs_per_interval.values)

# Add labels and title
plt.xlabel('Elevation Gain Interval (m)')
plt.ylabel('Frequency')
plt.title('Number of Runs vs. Elevation Gain Intervals')

# Rotate x-axis labels if needed
plt.xticks(rotation=40)

# Display the graph
plt.show()

# Sort the data frame based on a column
sorted_df = montseny_df.sort_values('elevation_gain',ascending='False')

# Print the top 3 rows based on the sorted column
top_3_rows = sorted_df.tail(5)
print(top_3_rows)
