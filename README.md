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
GET name-of-the-index/_search
```
Example: 
```
GET news_headlines/_search
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/105432767-8c216700-5c15-11eb-9ea2-ef74a3bc5f1b.png)

### Get accurate total number of hits 
To improve the response speed on large datasets, Elasticsearch limits the total count to 10,000 by default.  If you want an accurate total number of hits, use the following query. 

Syntax:
```
GET name-of-the-index/_search
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
![image](https://user-images.githubusercontent.com/60980933/105433287-8f692280-5c16-11eb-8ee5-0b9d61fda418.png)

### Get categories of news headlines
Syntax:
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

#### Get time range
When indexing a document, both HTTP verbs `POST` or `PUT` can be used. 

1) Use POST when you want Elasticsearch to autogenerate an id for your document. 

Syntax:
```
POST Name-of-the-Index/_doc
{
  "field": "value"
}
````
Example:
```
GET news_articles/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2013-04-12",
        "lte": "2013-07-12"
      }
    }
  }
}
```
Expected response from Elasticsearch:
![image](https://user-images.githubusercontent.com/60980933/101933971-2d8ab700-3b9a-11eb-99a4-7d34b9819050.png)

2) Use PUT when you want to assign a specific id to your document(i.e. if your document has a natural identifier - purchase order number, patient id, & etc).
For more detailed explanation, check out this [documentation](https://www.elastic.co/guide/en/elasticsearch/guide/current/index-doc.html) from Elastic! 

Syntax:
```
PUT Name-of-the-Index/_doc/id-you-want-to-assign-to-this-document
{
  "field": "value"
}
```
Example:
AGGREGATIOn to figure out which topic has been metnioned most frequently
```
GET news_articles/_search
{
  "query": {
    "match": { "category": "ENTERTAINMENT" }
  },
  "aggregations": {
    "my_sample": {

          "significant_text": { "field": "headline" 
        
      }
    }
  }
}
```
### Look up things in a category in certain date
When you index a document using an id that already exists, the existing document is overwritten by the new document. 
If you do not want a existing document to be overwritten, you can use the _create endpoint! 

With the _create Endpoint, no indexing will occur and you will get a 409 error message. 

Syntax:
```
PUT Name-of-the-Index/_create/id-you-want-to-assign-to-this-document
{
  "field": "value"
}
```
Example:
```
PUT favorite_candy/_create/1
{
  "first_name": "Finn",
  "candy": "Jolly Ranchers"
}
```

Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/101937947-cf60d280-3b9f-11eb-8341-316ec4a69b35.png)

## R - READ
### Read a document 
Syntax:
```
GET Name-of-the-Index/_doc/id-of-the-document-you-want-to-retrieve
```
Example:
```
GET favorite_candy/_doc/1
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/101935925-0d102c00-3b9d-11eb-9620-1b642364ef6a.png)

## U - UPDATE
### Update a document

If you want to update fields in a document, use the following syntax:
```
POST Name-of-the-Index/_update/id-of-the-document-you-want-to-update
{
  "doc": {
    "field1": "value",
    "field2": "value",
  }
} 
```
Example:
```
POST favorite_candy/_update/1
{
  "doc": {
    "candy": "M&M's"
  }
}
```
Expected response from Elasticsearch:

![image](https://user-images.githubusercontent.com/60980933/101938690-05528680-3ba1-11eb-8eec-8e2dce678405.png)

## D- DELETE
### Delete a document

Syntax:
```
DELETE Name-of-the-Index/_doc/id-of-the-document-you-want-to-delete
```
Example:
```
DELETE favorite_candy/_doc/1
```
Expected response from Elasticsearch:
![image](https://user-images.githubusercontent.com/60980933/101939174-dab4fd80-3ba1-11eb-93fe-de682853bae4.png)

## Take Home Assignment




