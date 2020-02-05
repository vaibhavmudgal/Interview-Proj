# Scraper-O-Vehicles
Scraping a complete website with 24k+ advertisements of vehicles on sale

# What it does:
Opens Chrome, goes onto the target website, crawls over to the required page and starts scraping the page extracting the required information.

Some values were not mentioned in the right place, so the script first searches for the required keyword, finds the complete sentence/paragraph and then breaks it into POS with the help of TextBlob and then picks up the required entry value.

It also checks for a new vehicle advertisement on the website and adds it to the database if it detects one.

# Modules Used:
* **Requests** - To request a url to get its code
From lxml using etree - Using etree to use xpath for searching for an element in the code
* **Selenium Web-Driver** - For automating the process and to resemble human behaviour which also helps in not getting blocked by the server
* **Beautiful Soup** - To scrape the current page after using selenium to reach the page.
* **Pandas** - To create a DataFrame of the collected data.
* **textBlob** - To extract only important keywords by breaking a whole sentence into Part Of Speech tags. (used to extract MOT expiry from a paragraph)
* **Time** - To calculate time spent(used in running the update code again after a given time) and to use sleep method to enact human behaviour.

# CODE HEADINGS AND EXPLANATION

# Function To Search MOT Expiry:
* This function only runs if the MOT expiry dates are not mentioned in the column on the page.
* Argument “**a**”: The sentence containing MOT expiry dates.
* Uses textBlob to break the sentence into POS tags to and picks up Noun phrases and joins it to give a final result.
* Can also pick up date formats like: dd/mm/yyyy or mm/yyyy.

# First Function That Scrapes The Pages And Returns A List “MAIN” Containing All The Scraped Data In It:
* Selenium opens the chrome browser and opens the website “https://www.usedcarsni.com"
* Clicks on the “**search**” button on the main page.
* Selects “**Most Recently Added**” option in the drop down menu.
* Starts scraping the pages(argument number_of_pages is the pages to scrape)
* Starts searching for the required values(Mileage, Location etc) and adds to the main list.
* At the end it clicks on the next page and continues with the process.

# Creating Dataframe And Removing Unwanted “\n”:
* After collecting data in the main list, it creates a data frame of the data which is easy to manipulate and use.
* In the raw data, “\n” is present in the values, so removing “\n” is also done in this step.



# Update Check Function And Second Function:
* **update_check** function extracts the name of the vehicle from the first page and checks it with the last 20-25 names of the vehicle in our DataFrame. If the name matches, it returns the index of the car matched. (most recently added cars page helps in checking for the updated vehicles)
* run_second function adds the vehicle data upto the returned index value by the update_check function. The steps involved in the run_second function are the same in the step number #3 and #4.
* Arguments in run_second function are time_to_run_update and iterations, here time_to_run_update is the time after which it will again check for updates and iterations is the number of times it will repeat the same update check.

# How To Use The Script: 
* Run run_first() to scrape and create an initial list of vehicles currently present on the site. Then run the next cell to create a dataframe of the data scraped.
* Run run_second(time in sec, iterations) to check for updates after time in sec and it will check for iterations number of times.
