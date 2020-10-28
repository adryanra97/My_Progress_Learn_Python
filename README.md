# Python_Find_Address_From_LongitudeLatitude
progress #1 of my project study in programming. Code for convert longitude latitude to find address using geopy. Using CSV as input for find address to many longlat data.

the code for preview so you dont need to download it

# Code start here
import pandas as pd
import geopy
from geopy.geocoders import Nominatim
from geopy.extra.rate_limiter import RateLimiter
import matplotlib.pyplot as plt
from tqdm.notebook import tqdm_notebook
pd.set_option('display.max_columns', None)
#change path to file directory
path="input.csv"
df = pd.read_csv(path,sep=";") #change sep if needed, sep is the char that separate between column
#input your csv column name for longitude and latitude column, make sure the char is same
#we change it to other colum name so it can run my code below
column_longitude=""
column_latitude=""
df = df.rename(columns = {column_longitude:"X",column_latitude:"Y"}) 
#make the new colum of longitude and latitude and use the standard format so it can be input to code
df['longlat'] = df['Y'].map(str) + ',' + df['X'].map(str)
#code for finding address base on longlat coordinate, and write it at new column "address"
locator = Nominatim(user_agent='myGeocoder', timeout=15)
rgeocode = RateLimiter(locator.reverse, min_delay_seconds=0.0001)
tqdm_notebook.pandas()
df['address'] = df['longlat'].progress_apply(rgeocode)
#now export it to new file
#change address_result for the output name you want
#for me i like to export it as excel, if you want to change to csv change "to_excel" to "to_csv", dont forget to change .xls to .csv
df.to_excel(r'address_result.xls', index = True, header=True)
