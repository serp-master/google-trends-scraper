<h1 align="center">
Google Trends Scraper
</h1>

## Table of Contents 
- [Intro](#intro)
- [Installation](#installation)
- [Examples](#examples)
  - [Example 1: Daily Trends Data](#example-1:daily-trends-data)
  - [Example 2: Interest Over Time](#example-2:interest-over-time)
  - [Example 3: Real-Time Trends](#example-3:real-time-trends)
  - [Example 4: Related Queries](#example-4:related-queries) 
- [Usage](#usage)

Google Trends Scraper is a great market research tool. Finding out which keywords people are searching for can be essential to understanding societal sentiments and polarizations. In addition to this, it can be beneficial to companies seeking to gain insight into category trends, consumer search queries, and their competitors' performance.

In this post, you’ll learn how to scrape google trends data using JavaScript.

## Intro

To scrape the Google trends data, we’ll be using the Google Trends API library. The library exposes Google Trends data through an API layer. It’s only intended for use in Node. 

## Installation

To start with, you’ll need to install the trends scraper package. You can access the Google Trends API documentation on NPM. Before that, let’s create a folder for this particular project. After that, you can access the folder from your terminal. 

When inside the folder, initialize NPM. You can initialize NPM by running npm init. Be sure to create an index.js file if one doesn't exist. Now you’re ready to install the trend scraper. To do that, run the following line of code in your terminal:

```npm i google-trends-api```

Once the package is installed, you can double-check your package.json to be sure. For this post, I’ve installed google-trends-api version 4.9.2. After this, we’ll create a Results folder to house all the results from the scraping job.

Now that the setup is complete, let’s go into the index.js file. The first thing you want to do is to import the packages you need. We’ll be using two packages for our examples: google trends api package and file system package. You can import the packages by entering this line of code in your index.js file:

```
const fs = require('fs');
const googleTrends = require('google-trends-api');
```

This code will help import the trends scraper tool and a file system package to save the results in JSON format.

## Examples

The scraper provides you with several access points and methods that you can call to help streamline the data. Here, we’ll be looking at some examples.

### Example 1: Daily Trends Data

The first example we’ll look at is how to scrape Google for daily trends. The dailyTrends() method showcases search traffic within a 24 hour period (hourly updates). In the search trends, you can check the frequency of specific searches and how many total searches occur in total. This method returns 20 daily trending results.

The following example block code displays the implementation of the daily trends scraper. 

```
googleTrends.dailyTrends({
    trendDate: new Date('2021-11-10'),
    geo: 'AU',
  }, function(err, results) {
    if (err) {
        console.log(err);
    }else{
        results = JSON.parse(results);
        const data = JSON.stringify(results.default || {}, null, 2);
        fs.writeFileSync(`Results/australiaTrend.json`, data);
    }
  })
  ``` 
  
Now let's look at the code more closely. For the dailyTrend() method, there is only one required parameter: geo. This parameter provides the geographical location of the search trend. In this example, we use ‘AU’, which stands for Australia. You can access other country codes here. 

In the example, we also include the optional parameter trendDate to signify the particular date to retrieve for the trend search. If you don’t supply a date, it defaults to the current date.

**Results**

Meanwhile, if there are no errors, the results are parsed into JSON format and stored in the **Results** folder we created earlier. 

## Example 2: Interest Over Time
 
There will be times when you need to compare the interest rates of two or more keywords over a period of time. This is especially crucial when conducting competitor analysis. The next example will be scraping Google trends data for interest over a particular period. 

The interestOverTime() method is used in this example. There is only one required parameter: keyword. The example will attempt to scrape trends data and gauge US interest on three keywords ( Donald Trump Vote, Barack Obama Vote, and Joe Biden Vote) from 2019 to 2021.

It is shown in the following code block:

```
googleTrends.interestOverTime({
    keyword: ['Donald Trump Vote', 'Joe Biden Vote', 'Barack Obama Vote'], 
    startTime: new Date('2019-01-01'), 
    endTime: new Date('2021-01-01'),
    geo: 'US'
})
.then(function(results){
    results = JSON.parse(results);
    const data = JSON.stringify(results.default, null, 2);
    fs.writeFileSync(`Results/Overtime-trends.json`, data);
})
.catch(function(err){
  console.error('Oh no there was an error', err);
});
```

**Results**

Three search trend results and queries from October and November 2020 shows the following:

```
   {
      "time": "1603584000",
      "formattedTime": "Oct 25 – 31, 2020",
      "formattedAxisTime": "Oct 25, 2020",
      "value": [
        12,
        20,
        0
      ],
      "hasData": [
        true,
        true,
        true
      ],
      "formattedValue": [
        "12",
        "20",
        "<1"
      ]
    },
    {
      "time": "1604188800",
      "formattedTime": "Nov 1 – 7, 2020",
      "formattedAxisTime": "Nov 1, 2020",
      "value": [
        68,
        100,
        3
      ],
      "hasData": [
        true,
        true,
        true
      ],
     "formattedValue": [
        "68",
        "100",
        "3"
      ]
    },
    {
      "time": "1604793600",
      "formattedTime": "Nov 8 – 14, 2020",
      "formattedAxisTime": "Nov 8, 2020",
      "value": [
        17,
        20,
        1
      ],
      "hasData": [
        true,
        true,
        true
      ],
      "formattedValue": [
        "17",
        "20",
        "1"
      ]
    },
```

## Example 3: Real-Time Trends

You can also access real-time trend reports over the last 24 hours. The following code block shows the implementation of the realTimeTrends() method.

```
googleTrends.realTimeTrends({
    geo: 'US',
    category: 'all',
}, function(err, results) {
    if (err) {
       console.log(err);
    } else {
        results=JSON.parse(results);
        const data = JSON.stringify(results, null, 2);
        fs.writeFileSync(`Results/USRealTimeTrend.json`, data);
    } 
});
```

## Example 4: Related Queries
 
In our final example, you’ll learn how to scrape search queries closely related to a keyword. This is highly crucial for data aggregation and mining. The relatedQueries() method will be used. This example examines the related queries to the keyword ‘Manchester United’. 
 
```
googleTrends.relatedQueries({keyword: 'Manchester United', startTime:new Date ('2021-11-01')}, function(err, results){
    if (err){
        console.log(err);
    } else {
        results=JSON.parse(results);
        const data = JSON.stringify(results, null, 2);
        fs.writeFileSync(`Results/RelatedQueries.json`, data);
    }
 
}
)
```

**Results** 
Three ranked results from the related queries example is shown below.

```
          {
            "query": "manchester united manchester city",
            "value": 100,
            "formattedValue": "100",
            "hasData": true,
            "link": "/trends/explore?q=manchester+united+manchester+city&date=2021-11-1+2021-11-19"
          },
          {
            "query": "manchester city",
            "value": 95,
            "formattedValue": "95",
            "hasData": true,
            "link": "/trends/explore?q=manchester+city&date=2021-11-1+2021-11-19"
          },
                 {
            "query": "manchester united city",
            "value": 92,
            "formattedValue": "92",
            "hasData": true,
            "link": "/trends/explore?q=manchester+united+city&date=2021-11-1+2021-11-19"
          },
```

## Usage

You can run each example by typing the following command in your terminal: 

```node index.js```


For further examples, be sure to check out the documentation [here.](https://github.com/pat310/google-trends-api)









