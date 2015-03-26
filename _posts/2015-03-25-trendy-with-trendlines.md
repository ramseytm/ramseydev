---
layout: post
title: "Getting trendy with trendlines in Kendo DataViz (Part One)"
description: "A simple overview of how to plot charts with Kendo DataViz and how to custom plot trendlines on those charts."
author: Tyler Ramsey
date: 2015-03-25 21:00
twitter_card: summary
header_image: http://tylerramsey.net/assets/201503/example_chart1.png
tags:
- development
- kendo
- javascript
---

<iframe src="//giphy.com/embed/JiL1EN1E5yriU?html5=true" class="header-image" width="480" height="224" frameBorder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>

<br />

Your mission, should you choose to accept it, is to plot a linear trendline for a series of data points using Kendo's DataViz. 'Sounds boring' you say? Yeah, I thought so too at first but it turned out to be an entertaining exercise that I thought I'd share. Full disclaimer, this a quickly thrown-together example for fun so take that for what it is...

# Data you say?

So to start out we're going to need some data to plot. Any data will do but I've been in a movie watching mood so I'm going to plot out the last decade of box office ticket sales and see how hollywood's been holding up. You can find the data required a few places, I picked [this one](http://pro.boxoffice.com/statistics/yearly "Yearly Box Office Sales"). I've no idea if it's accurate or not but [quickly-thrown together examples](http://blogs.msdn.com/b/oldnewthing/archive/2013/10/14/10456386.aspx "Little Programs") don't really care about accuracy.

So that data looks great. The next thing we need to do is put that data in a datasource that can be consumed by Kendo's DataViz chart control. To keep things simple I'm just going to use a JSON object we can use locally that looks like so:

{% highlight javascript %}
var ticketSalesPerYear = [
    {"year": 2004, "ticketsSold": 1496847021},
    {"year": 2005, "ticketsSold": 1374054602},
    {"year": 2006, "ticketsSold": 1400664122},
    {"year": 2007, "ticketsSold": 1399167151},
    {"year": 2008, "ticketsSold": 1341279944},    
    {"year": 2009, "ticketsSold": 1414014666},
    {"year": 2010, "ticketsSold": 1339733840},
    {"year": 2011, "ticketsSold": 1284440101},
    {"year": 2012, "ticketsSold": 1359472361},
    {"year": 2013, "ticketsSold": 1343018450},
    {"year": 2014, "ticketsSold": 1267354957}];
{% endhighlight %}

Nothing too crazy here, I'm just declaring an array varable containing an object for each year and that years' number of tickets sold. We'll use this array to plot a line graph using the [Kendo Chart](http://demos.telerik.com/kendo-ui/line-charts/index) control.

# Plotting it all together

Next we're going to plot a line graph that will serve as our base data for our trendline. To plot the line graph we'll need to initialize a kendo chart with our data array. The full working example including the kendo scripts required to run it looks like so:

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <link rel="stylesheet" href="http://cdn.kendostatic.com/2015.1.318/styles/kendo.common-material.min.css" />
    <link rel="stylesheet" href="http://cdn.kendostatic.com/2015.1.318/styles/kendo.material.min.css" />
    <link rel="stylesheet" href="http://cdn.kendostatic.com/2015.1.318/styles/kendo.dataviz.min.css" />
    <link rel="stylesheet" href="http://cdn.kendostatic.com/2015.1.318/styles/kendo.dataviz.material.min.css" />

    <script src="http://cdn.kendostatic.com/2015.1.318/js/jquery.min.js"></script>
    <script src="http://cdn.kendostatic.com/2015.1.318/js/kendo.all.min.js"></script>
</head>
<body>
<div id="example">
    <div id="chart"></div>
    <script>
        var ticketSalesPerYear = [
            {"year": 2004, "ticketsSold": 1496847021},
            {"year": 2005, "ticketsSold": 1374054602},
            {"year": 2006, "ticketsSold": 1400664122},
            {"year": 2007, "ticketsSold": 1399167151},
            {"year": 2008, "ticketsSold": 1341279944},
            {"year": 2009, "ticketsSold": 1414014666},
            {"year": 2010, "ticketsSold": 1339733840},
            {"year": 2011, "ticketsSold": 1284440101},
            {"year": 2012, "ticketsSold": 1359472361},
            {"year": 2013, "ticketsSold": 1343018450},
            {"year": 2014, "ticketsSold": 1267354957}];

        function createChart() {
            $("#chart").kendoChart({
                dataSource: {
                    data: ticketSalesPerYear
                },
                title: {
                    text: "Total Box Office Ticket Sales"
                },
                legend: {
                    visible: false
                },
                seriesDefaults: {
                    type: "line",
                    labels: {
                        visible: false,
                        format: "n0",
                        background: "transparent"
                    }
                },
                series: [{
                    field: "ticketsSold",
                }],
                valueAxis: {
                  labels: {
                    format: "n0" 
                  }
                },
                categoryAxis: {
                    field: "year",
                    majorGridLines: {
                        visible: false
                    },
                }
            });
        }

        $(document).ready(createChart);
    </script>
</div>
</body>
</html>

{% endhighlight %}

Again, this is fairly straight forward. We're adding the html required to get the example to run, creating our array and then generating a kendo chart on the div with the id "#chart" using our array as the data source's data parameter. I'm not going to get too in-depth with how the kendo chart works as you can find more detailed examples on their site. However, in general you pass it an array of objects, then define which field in the array will serve as the "series", define which field will serve as the "category axis" and then kendo does the rest. 

If you throw that code in an html page and open it up in your browser it should render something like this: 

[![example_chart1](http://tylerramsey.net/assets/201503/example_chart1.png "Click for js fiddle")](https://jsfiddle.net/ramseytm/d79q8p33/)
{: style="text-align: center"}

# Getting to the point

OK, so that looks great and we've got a base to work with. Now, let's say we want to demonstrate the over-all trend of the number of box office tickets sold in the past decade using a linear trendline. Kendo, at the time of writing, does not have a built-in way of generate a trendline like, say, Excel does. So we'll have to do it manually.

Fortunately, we've already demonstrated the Kendo chart can easily draw lines between the points in our series quite easily. So, all we have to do to draw a trendline is calculate the start and end point of the line and add it as a new series for the chart widget to draw for us. That's exactly what we'll do in the next post. 