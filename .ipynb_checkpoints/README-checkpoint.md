![img](https://cdn-images-1.medium.com/max/1600/1*N4ZNL18TtZ1ogq5-zDIDWA.jpeg)



Web Scraping is a technique employed to gather large amounts of data from websites whereby the data is extracted and stored into a structured format. such technique is widely used for a range of purposes, including market analysis, machine learning, news tracking and financial data aggregation.
The ability of web scraping is an important skill for any data scientist to have in their set.

There are many different approaches for getting data from the web such as writing a custom crawler from scratch, or by using one of the many existing libraries that make it absolutely easy to create a web scraping tool in Python.

In this Python tutorial we will go through a simple example of how to scrap a website to gather news articles and create simple applications using NEWSAPI library.

Most of the libraries for gathering information are Python standard, with the exception of the Pandas and NewsApi libraries accessible via installation and free registration on the official websites.



#### Prerequisites

###### Get API key

Firstly, get the API key by registering on the News API web page. Be sure to register as an individual. This will grant the free usage of the API. [Click Here](https://python.gotrained.com/news-api/)  and fill out the form. The API key should be in your inbox shorty.

<img src="/Users/boula/Library/Mobile Documents/com~apple~CloudDocs/Screenshots/Screen Shot 2021-06-11 at 11.27.32 PM.png" style="zoom:33%;" />

#### Import Libraries

```
import pandas as pd
import requests
import json
```

#### API Key

Note that in order to interact with the API, it is mandatory to provide the API key.

There are two ways to do that 

- Using API key in the request URL ("https://newsapi.org/v2/everything?q=bitcoin&apiKey=API_KEY?")
- newsapi = NewsApiClient(api_key='API_KEY')

#### Headers

we create a dictionary to store our API key, we include it in the header of the "requests"

```python
# Create a variable to hold the API key
headers = {'Authorization': 'cd23a40b7a0d44cb99af86eeb31869eb'}
```

#### API Endpoint

News API has 2 main endpoints:

- [Everything](https://newsapi.org/docs/endpoints/everything) `/v2/everything` – search every article published by over 75,000 different sources large and small in the last 3 years. This endpoint is ideal for news analysis and article discovery.
- [Top headlines](https://newsapi.org/docs/endpoints/top-headlines) `/v2/top-headlines` – returns breaking news headlines for countries, categories, and singular publishers. This is perfect for use with news tickers or anywhere you want to display live up-to-date news headlines.

```python
# create 2 variables to hold the API endpoints.
everything = "https://newsapi.org/v2/everything?"
top_headlines = "https://newsapi.org/v2/top-headlines?"
```

#### API Keywords

Keywords or a phrase to search for.

the parameter ‘***q***’ stands for the keyword search value. The news articles matching the keyword search are returned as a response

```python
# in our tutorial we look for news related to Apple
symbols = "apple"
```

#### API Sources

A comma-seperated string of identifiers for the news sources or blogs you want headlines from. 

```python
sources = ['abc-news', 'business-insider', 'financial-post', 'google-news',
           'reuters','nbc-news', 'techcrunch', 'the-wall-street-journal']
```

#### API sortBy

The ‘***sortBy***’ parameter contains the option that applies to the returned articles. Based on this value, the articles are sorted in the response

Possible options: [relevancy, popularity, publishedAt].

```python
sorby = "popularity"
```

#### Payloads

Next step is to create different payloads that need to be sent to the API. The payload is nothing more than some information that needs to be sent to the API. The payload can include information such as news category, country, language, etc.

```python
params= {
        'q': keywords,
        'apiKey': api_key,
        'sortBy': sorby,
        'language': 'en',
        'page': 1
        }
```

#### Requests API

Make the request using the get() method of the requests library.

Make the request using the get() method of the requests library.
we're going to pass ony 2 parameters, first the 'url' parameter,  specify the NewsAPI endpoint, and lastly pass the payloads dictionary to the 'params' parameter.

```python
response = requests.get(url=top_headlines, headers=headers, params=params)
```

The structure of the "***requests***" remains the same throughout, we can update the parameters "***url***" and "***params***" parameter in order to gather diffrent type of news.

#### Response

Create a viariable to hold the the "response"  in dictionary format

```python
output = response.json()
```

Print out the response on the console by accessing the method "json", since it already data formated by NewsAPI library.

```python
print(json.dumps(output, indent=4))
```

```
{
    "status": "ok",
    "totalResults": 10,
    "articles": [
        {
            "source": {
                "id": null,
                "name": "Rappler"
            },
            "author": "Rappler.com",
            "title": "Jollibee introduces new sweet chili chicken - Rappler",
            "description": "Jollibee's fried chicken is coated in a sweet-spicy chili glaze",
            "url": "https://www.rappler.com/life-and-style/food-drinks/jollibee-sweet-chili-chicken",
            "urlToImage": "https://assets2.rappler.com/2021/06/1-1623468746894.jpg",
            "publishedAt": "2021-06-12T03:39:00Z",
            "content": "Jollibee's new limited edition item can be ordered for dine-in, take-out, drive-thru, or delivery via the Jollibee Delivery app, website, hotline number, GrabFood, and foodpanda. Rappler.com"
        },
        {
            "source": {
                "id": null,
                "name": "Rappler"
            },
            "author": "Reuters",
            "title": "Hong Kong democracy activist Agnes Chow released from prison - Rappler",
            "description": "(1st UPDATE) Agnes Chow serves nearly seven months in prison for her role in an unauthorized assembly during Hong Kong's 2019 anti-government protests",
            "url": "https://www.rappler.com/world/asia-pacific/hong-kong-democracy-activist-agnes-chow-released",
            "urlToImage": "https://assets2.rappler.com/2021/06/Agnes-Chow-Wikimedia-June-12-2021-1623465814534.jpeg",
            "publishedAt": "2021-06-12T02:45:00Z",
            "content": "Hong Kong pro-democracy activist Agnes Chow was released from prison on Saturday, June 12, after serving nearly seven months for her role in an unauthorized assembly during anti-government protests i\u2026 [+18 chars]"
        },
```

#### Save Resonse to CSV

In order to create a dataframe with the result obtained, we must access the dictionary values using the "articles" key

```python
print(output.keys())
```

```
dict_keys(['status', 'totalResults', 'articles'])
```

Create a variable to hold the articles information.

```python
articles = output['articles']
```

Using "pandas" we create a dataframe using the method "DataFrame" passing parameter "data" with the articles variable

```python
df = pd.DataFrame(articles)
df.head(3)
```

![df](/Users/boula/Library/Mobile Documents/com~apple~CloudDocs/Screenshots/Screen Shot 2021-06-12 at 2.09.47 AM.png)

https://raw.githubusercontent.com/akladyous/newsapi-web-scraping/main/img/df.png?raw=True

Thanks for reading and happy web scraping everyone!

You can find my Jupyter Notebook for this on my [Github](https://github.com/akladyous/newsapi-web-scraping).