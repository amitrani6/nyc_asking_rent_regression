# nyc_asking_rent_regression

A project based on regression analysis of apartment listings to predict asking rents in New York City.

This project folder contains 14 files that will be summarized below. The process for this project was as follows:

1. Obtain rental posting data for New York City apartments from an outside source

2. Consolidate the data into a pandas dataframe

3. Perform exploratory data analysis (EDA) on the dataframe to find any relevant patters and characteristics to the regression

4. Perform regression analysis on the rental postings



Part 1: Obtain the data

I web scraped RentHop.com for rental postings in New York City (file: RentHop Web Scrape File.ipynb). The search results listed twenty postings per page by most recent posting first. Each individual posting contains a url to the specific posting site, the asking rent, the number of bedrooms and bathrooms, the address, the unit number, the borough, the neighborhood, and a list that contained miscellaneous data (including amenities).

I then accessed each individual posting's individual page to obtain further information regarding amenities and the nearest subway stop. I saved each posting as a list of dictionaries which was then saved as a pandas dataframe. This dataframe is saved as a csv file titled: cleaner_list_of_summary_rent_listings.csv.

I wanted to pair the posting data to economic data about the neighborhood. In the notebook titled 'Median Household Income Scrape.ipynb' I webscraped a site called 'incomebyzipcode.com' to obtain the median income and percent of residents who earn more than $200,000 per year in each neighborhood. I created the file 'dataframe_with_zipcodes_only.csv' which lists the neighborhood and corresponding zipcode for the purposes of browsing the incomebyzipcode site. The data is saved as csv file titled 'zip_code_info.csv'
nycchoropleth.html

from IPython.display import HTML
HTML(filename='nycchoropleth.html')
display(HTML('nycchoropleth.html'))
