# README:Geospacial Data Visualisation with Hover Fuctionality

Project Description 

README:Geospatial Data Visualisation with Hover Functionality

The script visualises geospatial data for donations, food wastage and hunger indices.
Focusing on Africa sub-regions on a map done using Basemap and seaborn. It plots data points colored, size-scaled bubbles and includes an interactive hover functionality to display region specific details donations amount by utilising mplcursor.

The code processes a merged DataFrame(merged_df) containing regional data, ensuring flexibility for different datasets.

Importance of learning Geospatial
Mastering geospatial visualisations is vital for data analysis in fields like environmental science and public policy.

Interactive features like hover do enhance user experience, making data exploration intuitive.
The project reinforces integration of mapping tools, data handling with pandas and interactive plotting, which are key skills to have for data science and visualisation.

# Table of Contents 

Installation
Usage
Preparing Data
Running the script
Code overview
Troubleshooting

1. Installation

To run this notebook locally, install the required python libraries. Use a virtue environment for best practice.

Prerequisites

Python 3.7 +
pip (python package manager)
Jupyter Notebook




# Steps

Clone or Download the notebook
Save the notebook to the local directory or most convenient way example on github.


Install Dependencies

Run the following command in your terminal 

pip install pandas matplotlib basemap seaborn mplcursors	


 Verify Installation 

Run this code in python to check:

import pandas, matplotlib, mpl_toolkits.basemap, seaborn, mplcursors
print("All libraries installed successfully!")

Optional: GUI Backend
Ensure your Matplotlib backend supports interactive plots example (TKAGG or Qt5Agg)
 
Test it:
	import matplotlib
print(matplotlib.get_backend())

If it’s not a GUI backend (e.g., `'Agg'`), switch it:

matplotlib.use('TkAgg') or (‘Qt5Agg’)

2. Usage

Preparing Your Data

The script expects a single, cleaned merged_df DataFrame containing following columns:
Region: Sub-region name example “East Africa”
donation _amount: Donation values example (a thousand dollars)
Wastage: Food wastage values example (ten tons)
hunger _index: Hunger index values example (0-1 scale)
Latitude,longitude: coordinates (30.0, 5.0)
 
Example Data Format:

region,donation_amount,wastage,hunger_index,latitude,longitude
North Africa,6324.54,25,0.25,30.0,5.0
Central Africa,5978.00,18,0.15,0.0,20.0

Running the Script

# Load data

donations_df = pd.read_csv("donations.csv")
hunger_df = pd.read_csv("hunger.csv")
food_wastage_df = pd.read_csv("food_wastage.csv")


         
merged_df = pd.merge(donations_df, hunger_df, on='region')
merged_df = pd.merge(merged_df, food_wastage_df, on='region')
print("Merged DataFrame:\n", merged_df)
  


Execute in a Python environment or Jupyter notebook

geospatial _donations-plot.ipynb

Three maps will display:Donations(blue), Food Wastage(red) and hunger(green)
Hover over points to see details 

Adjust parameters(optional)
Bubble Size: modify min_size, max_size, or scale_factor in the for loop.
Map Bounds: Change IIcrnrlat, urcrnrlat, llcrnrlon, urcrnrlon in Basemape for different regions.

3. Code Overview

- Data Preparation: Defines merged_df and maps coordinates using region_centroids.

- Data Structure: Uses dataframes dictionary to handle multiple datasets from on Dataframe.

- Plotting function (plot_geospatial_seaborn):
      - plots data on an Africa-focused map.
      - Scales bubble size based on values 
      - Adds a hover annotations with region and value details


 

def plot_geospatial_seaborn(df, title, color, label, scale_factor, value_column, min_size=50, max_size=500):
   lat = df['latitude'].values
   lon = df['longitude'].values
   try:
       values = df[value_column].values
   except KeyError:
       print(f"Error: Column '{value_column}' not found in DataFrame. Available columns: {df.columns.tolist()}")
       return
  
   if len(lat) == 0 or len(lon) == 0 or len(values) == 0:
       print(f"Skipping {title}: No data to plot.")
       return


   print(f"{title} - Data points to plot: {len(lat)}")


cursor = mplcursors.cursor(scatter, hover=True)
   @cursor.connect("add")
   def on_add(sel):
       idx = sel.target.index
       region = df['region'].iloc[idx]
       value = df[value_column].iloc[idx]
       print(f"Hover triggered at index {idx}: Region={region}, Value={value}")  # Debug hover
       sel.annotation.set_text(f"Region: {region}\n{label}: {value:.2f}")


   plt.show()
   print(f"{title} plot displayed.")

 
- Execution: Loops through datasets to generate individual plots 

 for file_name, (df, value_column) in dataframes.items(): 
   title = f"Regional {file_name.split('.')[0].capitalize().replace('_', (' '))} Geospatial Distribution "
   color = {'donations.csv': 'blue', 'food_wastage.csv': 'red', 'hunger.csv': 'green'}[file_name]
   label = {'donations.csv': 'Donations ($M)', 'food_wastage.csv': 'Wastage (tons)', 'hunger.csv': 'Hunger Index'}[file_name]
   scale_factor = {'donations.csv': 0.05, 'food_wastage.csv': 10, 'hunger.csv': 1000}[file_name]
   plot_geospatial_seaborn(df, title, color, label, scale_factor, value_column, min_size=50, max_size=300)
  

4. ​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​Troubleshooting

Hover Not Displaying
Check Backend:run 
matplotlib.use('TkAgg') or matplotlib.use('Qt5Agg')
Adjust with plt.switch_backend(‘Qt5Agg')
Console Output: Look for “Hover triggered …’ messages. If present but no popup, ensure a GUI environment(not a non-interactive terminal)
Check the console for “Skipping..” messages indicating empty data

-  Installation Errors: 
	- Python 3.9.21 version prefered 
	- Ensure all libraries are installed (pip list)
	- For basemap, use conda install basemap if pip falls

- Plot Not Showing
	- Run in an IDE example vs Code or Jupyter notebook for better interactivity 
	- Increase plt.pause(0.1) to (1) if required.
   

​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​Credits of code reference materials and other authors

- Data Science Skills Bootcamp - Q&A  31/01/2025 Live practice hosted by Bonaventure Gisemba.
- Use of Microsoft copilot, X Deepsearch for format, research and code debugging
- UN geospatial Data and simulation data 

