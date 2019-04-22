# NYC Asking Rent Regression Project

A project based on regression analysis of apartment listings to predict asking rents in New York City.

This project folder contains 14 files that will be summarized below. The process for this project was as follows:

1. Obtain rental posting data for New York City apartments from an outside source

2. Consolidate the data into a pandas dataframe

3. Perform exploratory data analysis (EDA) on the dataframe to find any relevant patters and characteristics to the regression

4. Perform regression analysis on the rental postings



### Part 1: Obtain the data

I web scraped RentHop.com for rental postings in New York City (file: RentHop Web Scrape File.ipynb). The search results listed twenty postings per page by most recent posting first. Each individual posting contains a url to the specific posting site, the asking rent, the number of bedrooms and bathrooms, the address, the unit number, the borough, the neighborhood, and a list that contained miscellaneous data (including amenities).

I then accessed each individual posting's individual page to obtain further information regarding amenities and the nearest subway stop. I saved each posting as a list of dictionaries which was then saved as a pandas dataframe. This dataframe is saved as a csv file titled: cleaner_list_of_summary_rent_listings.csv.

I wanted to pair the posting data to economic data about the neighborhood. In the notebook titled 'Median Household Income Scrape.ipynb' I web scraped a site called 'incomebyzipcode.com' to obtain the median income and percent of residents who earn more than $200,000 per year in each neighborhood. I created the file 'dataframe_with_zipcodes_only.csv' which lists the neighborhood and corresponding zipcode for the purposes of browsing the incomebyzipcode site. The data is saved as csv file titled 'zip_code_info.csv'
nycchoropleth.html

### Part 2: Consolidate the data into a pandas dataframe:

The purpose of this step was to adjust the data so that it can be easily manipulated for regression analysis. The problem with this data, as with all user-entered data, is that it is not perfectly uniform (i.e. data is incorrectly entered or omitted). This problem was most noticeable in the address, unit and amenities columns. Fortunately only the amenities column was necessary for my analysis. (I later drop the unit column as it was rarely filled out). I further realized that it is common for users of the RentHop website to repost the same listing multiple times, making it impossible to differentiate between the two postings (It is common for one building to have more than one unit available, however many of the outliers appeared to be reposts.

I begin this step in the ‘Main Dataframe Cleaning Work.ipynb’ notebook. I merged the postings dataframe (cleaner_list_of_summary_rent_listings.csv) with the demographics dataframe ('zip_code_info.csv') and proceeded to drop the columns unnecessary for regression (url and unit). I also dropped rows with omitted values in certain rows.

I created dummy columns for the neighborhood, borough and top twenty-five most frequently listed amenities for my regression. I further created a dummy column for whether or not the apartment was a Studio. I finally saved this dataframe as ‘neighfinal_listing_dataframe.csv’ and proceeded to step three.

### Part 3: Perform EDA

I begin the EDA process by creating a choropleth map using the Folium library to show where my rent postings are distributed throughout the city. I followed a guide written by Finn Qiao (https://towardsdatascience.com/visualizing-data-at-the-zip-code-level-with-folium-d07ac983db20) in a medium post and adjusted the code for my purposes. The file titled ‘nyczipcodetabulationareas.geojson’ contains neighborhood boundaries by zip code for NYC. I cut out only those neighborhoods where I had postings and plotted a frequency-based map (saved as 'nycchoropleth.html'):

![NYC Rent Postings Choropleth](/images-for-readme/choropleth.png)

I further created a series of pie charts to show the makeup of bedroom types.

![NYC Rent Postings Bedroom Count](/images-for-readme/bed.png)

Most apartment listings are for one and two bedroom apartments, and most one bedroom apartment listings are for true one bedroom apartments, not studio apartments.

![NYC Rent Postings Studio Count](/images-for-readme/studio_pie.png)

Furthermore the listings are distributed across the boroughs as follows:

|   Borough   |    Listings   |
| ----------- |:-------------:|
| Manhattan   |     4903      |
| Brooklyn    |     1026      |
| Queens      |      554      |


Part 4: Regression Analysis

My regression analysis is performed in the notebook titled 'Main Dataframe Regression Analysis.ipynb'.

I begin by viewing the distribution of asking rents in the dataframe. The mean asking rent is $3895.70 and the innerquartile range is $2625 to $4300, with a standard deviation of $850.

![NYC Rent Distribution](/images-for-readme/rent-distribution.png)

There is a high degree of kurtosis and the distribution is skewed heavily to the left. This is due in part to approximately 21 outliers with asking rents above $10,000.

I continue my analysis by viewing a scatterplot of asking rents to various other variables to see the correlation. It is not possible to view the standard matrix as my dataframe contains 153 columns.

Bedrooms and median income appeared to have a higher correlation to asking rents than the location of the nearest subway station.

![bedrooms_to_rent](/images-for-readme/bedrooms_to_rent.png)
Bedrooms to Rent

![median_income_to_rent](/images-for-readme/median_income_to_rent.png)
Median Income to Rent

Finally I began the ols regression on all of my variables, the results are below:

![first_ols](/images-for-readme/first_ols.png)

I had a 0.53 r^2 and a 0.519 adjusted r^2. At first I was disappointed, however I remembered that this was user-entered data where reposts are likely and impossible to differentiate.

I created a second ols with the insignifficant coefficients removed. The r^2 went down but the difference between the r^2 and adjusted r^2 thinned, indicating an improvement by removing these coefficients at the 95% confidence level.
