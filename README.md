# 311-open-data-analysis
Analysis of open data from NYC 311 reports, from
https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/7ahn-ypff. This repository
contains a Jupyter notebook that adds extra data to "Illegal Parking" reports:

- NYPD precinct
- City council district
- Borough (the built-in borough data is inconsistent)
- Whether the report resulted in an "action being taken" resolution or a "summons was issued" resolution

... then breaks down the results for several "Illegal Parking" complaint types, and writes the results to a set of
Google Sheets spreadsheets.

## Installation

To install all the prerequisites to get this notebook running, you'll probably need all of the following:

$ sudo pip install jupyter
$ sudo pip install numpy
$ sudo pip install pandas
$ sudo pip install fiona
$ sudo pip install shapely
$ sudo pip install jupyter
$ sudo pip install rtree
$ sudo pip install tqdm
$ sudo pip install pygsheets
$ jupyter nbextension enable --py widgetsnbextension
$ jupyter labextension install @jupyter-widgets/jupyterlab-manager

## Data preparation

First, download the data from
https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/7ahn-ypff. It should produce a
file 311_Service_Requests_from_2010_to_Present.csv; put that in the illegal_parking/data directory. Then:

$ cd illegal_parking/data
$ grep "Illegal Parking" 311_Service_Requests_from_2010_to_Present.csv >> illegal_parking.csv

## Google Sheets preparation

If you're going to run the cells that automatically write data to Google Sheets, you'll need to create sheets with the
right names first: `311 "Blocked Bike Lane" reports`, for example, is where the script will look to write
data on "Blocked Bike Lane" reports.

You'll also need to follow the directions at https://pygsheets.readthedocs.io/en/stable/authorization.html to create a
client_secret.json for pygsheets.

## Notebook

Start the notebook:

$ jupyter notebook

1. Open illegal_parking/311_illegal_parking.ipynb.
2. Change the file paths for `client_secret.json`, `precinct_shapes`, and `council_district_shapes` to point to the
   right directories on your computer.
3. Uncomment the code in the cell that starts with "# Regenerate enhanced data".
4. Run all the cells under "Read data and add calculated columns" (which will probably take 4-6 hours, due to the
   heavy GIS calculations for NYPD precinct and city council district).
5. Comment out the code in the cell that starts with "# Regenerate enhanced data".
4. Run the cells in "Calculating 1d breakdowns". Note: The "Calculating 2d breakdowns" section also works just fine,
   but produces spreadsheets that didn't turn out to be as useful.
5. If everything is working, you should see data being written to the spreadsheets you created.
