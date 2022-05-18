# Beginner's Crash Course to Elastic Stack Series
## Part 2: Understanding the relevance of your search with Elasticsearch and Kibana

Welcome to the Beginner's Crash Course to Elastic Stack!

This repo contains all resources shared during Part 2: Understanding the relevance of your search with Elasticsearch and Kibana.

By the end of this workshop, you will: 
- learn how Precision and Recall are used to measure how well Elastic search engine is searching
- understand how scoring is used to rank the relevance of your search results in Elasticsearch
- master how to send search queries from Kibana to Elasticsearch to finetune Precision or Recall of your search results

## Resources

[Beginner's Crash Course to Elastic Stack Table of Contents](https://github.com/LisaHJung/Beginners-Crash-Course-to-the-Elastic-Stack-Series) This workshop is a part of the Beginner's Crash Course to Elastic Stack series. Check out this table contents to access all the workshops in the series thus far. This table will continue to get updated as more workshops in the series are released! 

[Free Elastic Cloud Trial](https://www.elastic.co/cloud/cloud-trial-overview/30-days?ultron=community-beginners-crash-course-May2022+&hulk=30d)

[Instructions](https://dev.to/lisahjung/beginner-s-guide-to-setting-up-elasticsearch-and-kibana-with-elastic-cloud-1joh) on how to access Elasticsearch and Kibana on Elastic Cloud

[Instructions](https://dev.to/elastic/downloading-elasticsearch-and-kibana-macos-linux-and-windows-1mmo) for downloading Elasticsearch and Kibana

[Video](https://www.youtube.com/watch?v=CCTgroOcyfM) of the workshop

[Mini Beginner's Crash Course to Elasticsearch & Kibana playlist](https://ela.st/mini-beginners-crash-course)

Do you prefer learning by watching shorter videos? Check out this playlist to watch short clips of beginner's crash course full length workshops. Part 2 workshop is broken down into episodes 7-10. Season 2 clips will be uploaded here in the future! 

[Presentation](https://github.com/LisaHJung/Part-1.2-Understanding-the-relevance-of-your-search-with-Elasticsearch-and-Kibana-/blob/main/Part%201.2%20Understanding%20the%20relevance%20of%20your%20search%20with%20Elasticsearch%20and%20Kibana%20-%20Beginner's%20Crash%20Course%20to%20the%20Elastic%20Stack%20Series%20-%20.pdf)

[News Headlines Dataset](https://www.kaggle.com/rmisra/news-category-dataset)

[Blog](https://dev.to/lisahjung/beginner-s-guide-to-understanding-the-relevance-of-your-search-with-elasticsearch-and-kibana-29n6) on understanding the relevance of your search with Elasticsearch and Kibana

[Elastic America Virtual Chapter](https://community.elastic.co/amer-virtual/) Want to attend live workshops? Join the Elastic America Virtual Chapter to get the deets!

[What's next?](https://github.com/LisaHJung/Part-3-Running-full-text-queries-and-combined-queries-with-Elasticsearch-and-Kibana) Eager to continue your learning after mastering the concept from this workshop? Move on to Part 3: Running full text queries and combined queries with Elasticsearch and Kibana [here](https://github.com/LisaHJung/Part-3-Running-full-text-queries-and-combined-queries-with-Elasticsearch-and-Kibana)!

## Search for information
There are two main ways to search in Elasticsearch:
1) Queries
2) Aggregations

### Queries
Queries retrieve documents that match the criteria. 

#### Retrieve information about documents in an index

Syntax: 
```
GET enter_name_of_the_index_here/_search
```
Example: 
```
GET news_headlines/_search
```
Expected response from Elasticsearch:

Elasticsearch displays a number of hits and a sample of 10 search results by default.  

![image](https://user-images.githubusercontent.com/60980933/105432767-8c216700-5c15-11eb-9ea2-ef74a3bc5f1b.png)

#### Get the exact total number of hits 
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

You will see that the total number of hits is now 200,853.

![image](https://user-images.githubusercontent.com/60980933/105531896-3c8b7b80-5ca7-11eb-949d-4a65ef0b3be1.png)

#### Search for data within a specific time range
Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "query": {
    "Specify the type of query here": {
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

### Aggregations

An aggregation summarizes your data as metrics, statistics, and other analytics. 

#### Analyze the data to show the categories of news headlines in our dataset
Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "aggs": {
    "name your aggregation here": {
      "specify aggregation type here": {
        "field": "name the field you want to aggregate here",
        "size": state how many buckets you want returned here
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

### A combination of query and aggregation request

#### Search for the most significant term in a category

Syntax:
```
GET enter_name_of_the_index_here/_search
{
  "query": {
    "match": {
      "Enter the name of the field": "Enter the value you are looking for"
    }
  },
  "aggregations": {
    "Name your aggregation here": {
      "significant_text": {
        "field": "Enter the name of the field you are searching for"
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
      "category": "ENTERTAINMENT"
    }
  },
  "aggregations": {
    "popular_in_entertainment": {
      "significant_text": {
        "field": "headline"
      }
    }
  }
}
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/105541764-7c595f80-5cb5-11eb-86e7-ffa44ba18d74.png)

### Precision and Recall

#### Increasing Recall

Syntax:
```
GET enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "Specify the field you want to search": {
        "query": "Enter search terms"
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
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner"
      }
    }
  }
}
```
Expected response from Elasticsearch: 

By default, the match query uses an "OR" logic. If a document contains one of the search terms, Elasticsearch will consider that document as a hit. 

"OR" logic results in higher number of hits, thereby increasing recall. However, the hits are loosely related to the query and lowering precision as a result.   

![image](https://user-images.githubusercontent.com/60980933/105553748-3d320b00-5cc3-11eb-9aeb-a9970c60f4fc.png)

#### Increasing Precision
We can increase precision by adding an "and" operator to the query. 

Syntax:
```
GET enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "Specify the field you want to search": {
        "query": "Enter search terms",
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
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "operator": "and"
      }
    }
  }
}
```
Expected response from Elasticsearch:

"AND" operator will result in getting more precise matches, thereby increasing precision. However, it will reduce the number of hits returned, resulting in lower recall. 

![image](https://user-images.githubusercontent.com/60980933/105552915-e24be400-5cc1-11eb-8881-4f6534cc6aa8.png)

#### minimum_should_match
This parameter allows you to specify the minimum number of terms a document should have to be included in the search results. 

This parameter gives you more control over fine tuning precision and recall of your search. 

Syntax:
```
GET enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Enter search term here",
        "minimum_should_match": Enter a number here
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
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "minimum_should_match": 3
      }
    }
  }
}
```
Expected response from Elasticsearch:

With minimum_should_match parameter, we were able to finetune both precision and recall! 

![image](https://user-images.githubusercontent.com/60980933/105939135-cde74e80-6015-11eb-9f3e-6a38cc373de2.png)

