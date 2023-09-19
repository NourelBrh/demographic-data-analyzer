# demographic-data-analyzer
import pandas as pd

# Read the dataset
data = pd.read_csv("adult.data.csv")

# 1. How many people of each race are represented in this dataset?
race_counts = data['race'].value_counts()

# 2. What is the average age of men?
average_age_men = data[data['sex'] == 'Male']['age'].mean()

# 3. What is the percentage of people who have a Bachelor's degree?
percentage_bachelors = (data['education'] == 'Bachelors').mean() * 100

# 4. What percentage of people with advanced education make more than 50K?
higher_education = data['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
percentage_higher_education = (data[higher_education]['salary'] == '>50K').mean() * 100

# 5. What percentage of people without advanced education make more than 50K?
percentage_lower_education = (data[~higher_education]['salary'] == '>50K').mean() * 100

# 6. What is the minimum number of hours a person works per week?
min_work_hours = data['hours-per-week'].min()

# 7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
num_min_workers = len(data[data['hours-per-week'] == min_work_hours])
rich_percentage = (data[(data['hours-per-week'] == min_work_hours) & (data['salary'] == '>50K')]['salary'].count() / num_min_workers) * 100

# 8. What country has the highest percentage of people that earn >50K and what is that percentage?
country_stats = data.groupby('native-country')['salary'].apply(lambda x: (x == '>50K').mean() * 100).sort_values(ascending=False)
highest_earning_country = country_stats.idxmax()
highest_earning_percentage = country_stats.max()

# 9. Identify the most popular occupation for those who earn >50K in India.
indian_occupation_stats = data[(data['native-country'] == 'India') & (data['salary'] == '>50K')]['occupation'].value_counts()
top_IN_occupation = indian_occupation_stats.idxmax()

# Output the results
print("1. How many people of each race are represented in this dataset?")
print(race_counts)

print("\n2. What is the average age of men?")
print(round(average_age_men, 1))

print("\n3. What is the percentage of people who have a Bachelor's degree?")
print(round(percentage_bachelors, 1))

print("\n4. What percentage of people with advanced education make more than 50K?")
print(round(percentage_higher_education, 1))

print("\n5. What percentage of people without advanced education make more than 50K?")
print(round(percentage_lower_education, 1))

print("\n6. What is the minimum number of hours a person works per week?")
print(min_work_hours)

print("\n7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?")
print(round(rich_percentage, 1))

print("\n8. What country has the highest percentage of people that earn >50K and what is that percentage?")
print(f"{highest_earning_country}: {round(highest_earning_percentage, 1)}%")

print("\n9. Identify the most popular occupation for those who earn >50K in India.")
print(top_IN_occupation)
