# Beginner's Crash Course to the Elastic Stack Series
## Part 1.2: Understanding the relevance of your search with Elasticsearch and Kibana

Welcome to the Beginner's Crash Course to the Elastic Stack!

This repo contains all resources shared during the workshop 1.2: Understanding the relevance of your search with Elasticsearch and Kibana.

## Resources

[Free Elastic Cloud Trial](https://ela.st/elastic-beginners)

[Instructions](https://dev.to/elastic/downloading-elasticsearch-and-kibana-macos-linux-and-windows-1mmo) for downloading Elasticsearch and Kibana

[Presentation]()

[Dataset](https://www.kaggle.com/rmisra/news-category-dataset) from Kaggle used for demo

[Elastic Austin User Group](https://www.meetup.com/elastic-austin-user-group/members/) Want to attend live workshops? Join the Elastic Austin User Group to keep up to date on all future events!

### Retrieve all documents from an index
Syntax: 
```
GET enter_name_of_the_index_here/_search
```
Example: 
```
GET news_headlines/_search
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/105432767-8c216700-5c15-11eb-9ea2-ef74a3bc5f1b.png)

### Get the exact total number of hits 
To improve the response speed on large datasets, Elasticsearch limits the total count to 10,000 by default.  If you want the exact total number of hits, use the following query. 

Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "track_total_hits": true
}
```
Example:
```
GET news_headlines/_search
{
  "track_total_hits": true
}
```
Expected response from Elasticsearch:
![image](https://user-images.githubusercontent.com/60980933/105531896-3c8b7b80-5ca7-11eb-949d-4a65ef0b3be1.png)

#### Search for data within a specific time range
Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "query": {
    "Enter the type of query here": {
      "Enter name of the field here": {
        "gte": "Enter lowest value of the range here",
        "lte": "Enter highest value of the range here"
      }
    }
  }
}
````
Example:
```
GET news_headlines/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2015-06-20",
        "lte": "2015-09-22"
      }
    }
  }
}
```
Expected response from Elasticsearch:
It will pull up articles published from June 20, 2015 through September 22, 2015. A document from the result set was shown as an example.

![image](https://user-images.githubusercontent.com/60980933/105539632-41096180-5cb2-11eb-917f-85f9ba01073e.png)

### Get categories of news headlines
Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "aggs": {
    "name your aggregation here": {
      "state your aggregation type here": {
        "field": "name the field you want to aggregate here",
        "size": state how many results you want returned here
      }
    }
  }
}
```
Example:
```
GET news_headlines/_search
{
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category",
        "size": 100
      }
    }
  }
}
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/105434428-cc361900-5c18-11eb-9db7-e7441ac5a1ac.png)

### Search for the most popular topic in a certain category

Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "query": {
    "match": { "Enter the name of the field": "Enter the value you are looking for" }
  },
  "aggregations": {
    "my_sample": {
       "significant_text": { "field": "Enter the name of the field you are searching for" }
    }
  }
}
```
Example:
```
GET news_headlines/_search
{
  "query": {
    "match": { "category": "ENTERTAINMENT" }
  },
  "aggregations": {
    "my_sample": {
       "significant_text": { "field": "headline" }
    }
  }
}
```
Expected response from Elasticsearch:
![image](https://user-images.githubusercontent.com/60980933/105541764-7c595f80-5cb5-11eb-86e7-ffa44ba18d74.png)

#### How do we increase recall?
Syntax:
```
GET enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "Specify the field you want to search":{
        "query":"Enter search terms"
   }
  }
 }
}
```
Example:
```
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline":{
        "query":"Khloe Kardashian Kendall Jenner"
   }
  }
 }
}
```
Expected response from Elasticsearch: 
![image](https://user-images.githubusercontent.com/60980933/105552502-3b674800-5cc1-11eb-8d5d-88f32d9beefa.png)

#### How do we increase Precision?
Syntax:
```
GET enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "Specify the field you want to search":{
        "query":"Enter search terms",
        "operator": "and"
   }
  }
 }
}
```

Example: 
```
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline":{
        "query":"Khloe Kardashian Kendall Jenner",
        "operator": "and"
   }
  }
 }
}
```
Expected response from Elasticsearch:
![image](https://user-images.githubusercontent.com/60980933/105552915-e24be400-5cc1-11eb-8881-4f6534cc6aa8.png)


## Take Home Assignment
1. Pick a time range you want to pull 




