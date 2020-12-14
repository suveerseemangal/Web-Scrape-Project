# Web-Scrape-Project
Learning how to make a web scraper
import requests
from requests import get 
from bs4 import BeautifulSoup
import pandas as pd 
import numpy as np
headers = {"Accept-Language": "en-US, en;q=0.5"}
url= "https://www.imdb.com/search/title/?groups=top_1000&ref_=adv_prv"
results = requests.get(url, headers=headers)
soup = BeautifulSoup(results.text,"html.parser")
print(soup.prettify())
#initalize empty lists where you'll store your data
titles =[]
years = []
time= []
imdb_ratings = []
metascores = []
votes=[]
us_gross = []
movie_div = soup.findAll('div', class_='lister-item mode-advanced')
for container in movie_div:
    #Name
    name=container.h3.a.text
    titles.append(name)
    
    #year
    year= container.h3.find('span', class_="lister-item-year").text
    years.append(year)
    
    #Time
    runtime= container.p.find('span', class_='runtime').text if container.p.find('span', class_='runtime').text else '-'
    time.append(runtime)
    
    #IMDb Rating 
    imdb = float(container.strong.text)
    imdb_ratings.append(imdb)
    
    #Metascore 
    m_score = container.find('span', class_='metascore').text if container.find('span', class_='metascore') else '-'
    metascores.append(m_score)
    
    #here are two NV containers, grab both of them as they hold both the votes and the grosses 
    nv= container.findAll('span', attrs={'name':'nv'})
    
    #filter Nv for votes 
    vote= nv[0].text
    votes.append(vote)
    
    #filter nv for gross 
    grosses =nv[1].text if len(nv)>1 else '-'
    us_gross.append(grosses)
   print (titles,years,time,imdb_ratings,metascores,votes,us_gross)
    movie_data = [titles,years,time,imdb_ratings,metascores,votes,us_gross]
     column = ['Titles','Year Published','IMDB Rating','Meta Score','US Gross']
    df= pd.DataFrame(movie_data)
    print (df)
