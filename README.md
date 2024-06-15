# Web Scraping with BeautifulSoup and Pandas
This repository contains Python scripts for web scraping using BeautifulSoup and Pandas. The scripts demonstrate how to scrape product details from an e-commerce site and table data from a webpage.

## Prerequisites
Install the required libraries using pip:

```pip install requests beautifulsoup4 pandas lxml```

# Scripts
## 1. Scrape Product Details
Scrapes product names, prices, descriptions, and reviews from an e-commerce site and saves them to a CSV file.

``` import pandas as pd
import requests
from bs4 import BeautifulSoup

url = "https://webscraper.io/test-sites/e-commerce/allinone/computers/tablets"
r = requests.get(url)
soup = BeautifulSoup(r.text, "lxml")

product_name = [i.text for i in soup.find_all("a", class_="title")]
price_list = [i.text for i in soup.find_all("h4", class_="price float-end card-title pull-right")]
descr_list = [i.text for i in soup.find_all("p", class_="description card-text")]
revw_list = [i.text for i in soup.find_all("p", class_="review-count float-end")]

df = pd.DataFrame({
    "Product Name": product_name,
    "Price": price_list,
    "Description": descr_list,
    "Reviews": revw_list
})

df.to_csv("product_details.csv", index=False)
```

## 2. Extract Nested Tags
Extracts data from nested HTML tags.

```import requests
from bs4 import BeautifulSoup

url = "https://webscraper.io/test-sites/e-commerce/allinone/computers/tablets"
r = requests.get(url)
soup = BeautifulSoup(r.text, "lxml")

box = soup.find_all("div", class_="col-md-4 col-xl-4 col-lg-4")[2]
name = box.find("a", class_="title").text
price = box.find("h4", class_="price float-end card-title pull-right").text
descr = box.find("p", class_="description card-text").text
revw = box.find("p", class_="review-count float-end").text

print(name, price, descr, revw)
```

## 3. Scrape Table Data
Scrapes table data from a webpage and saves it to a CSV file.

``` import pandas as pd
import requests
from bs4 import BeautifulSoup

url = "https://www.iplt20.com/auction"
r = requests.get(url)
soup = BeautifulSoup(r.text, "lxml")

table_fetch = soup.find("table", class_="table table-sm table-hover screenertable")
headers = [i.text.strip() for i in table_fetch.find_all("th")]

df = pd.DataFrame(columns=headers)
rows = table_fetch.find_all("tr")[1:]
for row in rows:
    data = [td.text.strip() for td in row.find_all("td")]
    df.loc[len(df)] = data

df.to_csv("stock_market_data.csv", index=False)
```

# Usage
Clone the repository.
Install the required libraries.
Run the scripts using Python:

```python scrape_products.py
python scrape_nested_tags.py
python scrape_table.py```

The scraped data will be saved in respective CSV files.




