# Electric-Road-Project
## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Collection](#data-collection)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Project Overview
This project aims to revolutionize transportation in Hikkaduwa by introducing a real-time electric road system for tourists along the Colombo-Galle Road. The project dynamically adjusts vehicle fares based on tourist input and analyzes potential profits, demand, and environmental impact.

## Data Sources
The project collects location data through a custom Streamlit form, allowing users to input key details for analysis. The dataset includes the following fields:

- Traffic Volume: Daily or weekly vehicle or pedestrian count.
- Rent Cost: Monthly rent for the location in monetary terms.
- Tourist Attraction Level: A score indicating the area's appeal to tourists.
- Distance from Town: Proximity to the nearest major town in kilometers.
- Competitor Presence: Count of similar businesses in the area.
  
All data is stored in a CSV file (location_data.csv) for analysis and visualization.

## Tools

- Vehicle Type (Car, Taxi, Bus, Van)
- Price Range
- Time of Day Preferences
- Monthly Travel Trends
- Feelings

## Data Collection:

- A custom Streamlit form gathers inputs.
- The data is saved in a structured excel format for processing.

## Data Analysis
``` python
# Import libraries
import streamlit as st
import pandas as pd
from datetime import datetime
from textblob import TextBlob
from openpyxl import load_workbook
from openpyxl.utils.dataframe import dataframe_to_rows

# File to store user preferences
DATA_FILE = "user_preferences.xlsx"


# Function to perform sentiment analysis
def analyze_sentiment(feedback):
    blob = TextBlob(feedback)
    sentiment_score = blob.sentiment.polarity  # Polarity ranges from -1 (negative) to 1 (positive)

    if sentiment_score > 0.3:
        return "Excited"
    elif sentiment_score < -0.3:
        return "Unhappy"
    else:
        return "Unsuitable for Sri Lanka"


# Function to save data to Excel
def save_to_excel(dataframe, file_path):
    try:
        # Load existing workbook if it exists
        workbook = load_workbook(file_path)
        sheet = workbook.active

        # Append new data to the Excel file
        for row in dataframe_to_rows(dataframe, index=False, header=False):
            sheet.append(row)

        workbook.save(file_path)
    except FileNotFoundError:
        # Create a new workbook and save the data
        with pd.ExcelWriter(file_path, engine="openpyxl") as writer:
            dataframe.to_excel(writer, index=False, sheet_name="Preferences")


# Set the page layout and configuration
st.set_page_config(page_title="Tourist Preferences for Electric Vehicle Project", layout="wide")

# Add a single-line title at the top of the page
st.markdown(
    "<h1 style='text-align: center; margin-top: 0;'>Tourist Preferences for Electric Vehicle Project</h1>",
    unsafe_allow_html=True
)

# Section 1: Preferred Vehicle Type
st.subheader("1. Preferred Vehicle Type")
vehicle_types = ["Car", "Taxi", "Bus", "Van"]
preferred_vehicle = st.selectbox("Select your preferred vehicle type:", vehicle_types)

# Section 2: Price Range Preferences
st.subheader("2. Price Range Preferences")
price_ranges = [
    "$5 - $20", "$21 - $50", "$51 - $100", "$101 - $200",
    "$201 - $300", "$301 - $400", "$401 - $500",
]
selected_price_range = st.selectbox(
    f"Select the price range for {preferred_vehicle} (in USD):",
    options=price_ranges
)

# Section 3: Month of Travel
st.subheader("3. Month of Travel")
months = [
    "January", "February", "March", "April", "May", "June",
    "July", "August", "September", "October", "November", "December"
]
preferred_month = st.selectbox("Select the month you prefer to visit Hikkaduwa:", months)

# Section 4: Time of Travel
st.subheader("4. Time of Travel")
time_of_day = ["Morning", "Afternoon", "Evening"]
preferred_time = st.radio("Select your preferred time of travel:", time_of_day)

# Section 5: User Feelings
st.subheader("5. Share Your Feelings")
feelings = st.text_area("How do you feel about this electric vehicle service?")

# Section 6: Submit and Save Data
if st.button("Submit"):
    # Analyze user sentiment
    sentiment = analyze_sentiment(feelings)

    # Create a dictionary with the user's preferences
    user_data = {
        "Timestamp": [datetime.now().strftime("%Y-%m-%d %H:%M:%S")],
        "Preferred Vehicle": [preferred_vehicle],
        "Price Range": [selected_price_range],
        "Preferred Month": [preferred_month],
        "Preferred Time": [preferred_time],
        "Feelings": [feelings],
        "Sentiment": [sentiment]
    }

    # Convert to a DataFrame
    df = pd.DataFrame(user_data)

    # Save data to Excel
    save_to_excel(df, DATA_FILE)

    st.success("Your preferences and feedback have been saved! The dashboard will update shortly.")
    st.write("Thank you for your input!")

```
## Results

1.Vehicle Demand Analysis:
The most preferred vehicle type is the bus (32.69% demand), followed by vans (25%), taxis (22.12%), and cars (20.19%).

2.Price Range Distribution:
Majority of vehicle usage falls under the lower price ranges of $5-$101, indicating tourists prefer economical travel options.

3.Estimated Price by Vehicle Type:
Buses generate the highest estimated revenue, significantly outperforming other vehicle types like vans, cars, and taxis.

4.Monthly Travel Trends:
A declining trend is observed in travel demand from June to November, suggesting seasonality in tourist activities.

5.Sentiment Analysis:
Most customers feel excited (35.58%) and happy (33.65%), while some express being unsuitable (29.81%) or unhappy (minimal).


## Recommendations
1.Focus on Bus Services:
- As buses dominate demand and revenue, ensure ample availability and superior service quality.
 Introduce tailored bus services like premium tourist packages to maximize profit.

2.Optimize Pricing:
- Keep fares within the $5-$101 range, as this aligns with most tourists' affordability.

3.Seasonal Strategies:
- Develop targeted marketing campaigns during peak seasons (June to October) to attract more tourists.
- Consider discounted rates or promotions during off-peak months like November.

4.Enhance Sentiment:
- Address negative feedback by improving vehicle comfort and services for cars, taxis, and vans.
- Leverage positive sentiment to promote the electric road experience.
## Limitations
- Analysis relies on a snapshot of data, which may not fully represent all tourist preferences.
- Seasonal trends may need further validation with larger datasets.
## Conclusion
The Electric Road Project in Hikkaduwa demonstrates significant potential to revolutionize eco-friendly tourism. The analysis highlights buses as the most profitable and demanded vehicle type, with pricing playing a crucial role in tourist satisfaction. While the project successfully captures key insights, enhancing real-time updates and addressing customer feedback can further optimize operations and promote sustainable travel.














