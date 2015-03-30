---
layout: post
title: "Getting trendy with trendlines in Kendo DataViz (Part Two)"
description: "The exciting conclusion to plotting linear trendlines in Kendo DataViz Chart controls."
author: Tyler Ramsey
date: 2015-03-29 21:00
twitter_card: summary
header_image: http://tylerramsey.net/assets/201503/example_chart2.png
include_math: true
tags:
- development
- kendo
- javascript
---

[![example_chart2](http://tylerramsey.net/assets/201503/example_chart2.png "Click for js fiddle")](https://jsfiddle.net/ramseytm/y2o9wx7j/1/)
{: style="text-align: center"}

In my [last post](http://tylerramsey.net/2015/03/25/trendy-with-trendlines.html) we left off with having plotted the past decade of box office ticket sales using the [Kendo Chart](http://demos.telerik.com/kendo-ui/line-charts/index) control. With our base data defined and plotted we're now going to plot a linear trendline on the chart like the one you see in the image above. Again, this is a quickly thrown-together example for fun so take that for what it is... 

# Lines, points & coordinates oh my!

So you may recall from your early education that a straight line can be defined in slope-intercept form by the following equation: 

$$ y = mx + b $$

In case you don't recall it, in the above equation $$ y $$ is the y-coordinate of a point on the line, $$ m $$ is the slope of the line, $$ x $$ is the x-coordinate of a point on the line and $$ b $$ is y-intercept. As you may have noticed a linear trendline is a straight line (hence *linear*) so we can use this equation to plot it on our Kendo Chart.

In order to plot the trendline we need to calculate the start and end points of the line we want the chart widget to draw. Using the equation above we can break down the steps required for finding the start and end points as follows:

1. Determine $$ x $$, in our case it will be 1 for start point and 11 for the end point since we're starting with the first year and ending with the eleventh.
2. Calculate the slope $$ m $$.
3. Calculate the y-intercept $$ b $$.
4. Solve for $$ y $$.
5. Add the $$ y $$ value as a new series in the first year and eleventh year records in our data source.

So we already know the $$ x $$ values we want to get the corresponding $$ y $$ values for so let's move on to calculating the slope.

# Calculating the slope: $$ m $$

So first off let's write a function that gets us the slope of a data set. The equation for calculating a the slope of a linear trendline over a set of data is the following:

<div class="eq-wrapper">
$$ m = [n∑xy – (∑x)(∑y)] / [n(∑x²) – (∑x)²] $$
</div>

If you're like me, upon first viewing that equation means nothing to you so let's get our hands dirty with some code. The above equation can be expressed in javascript as follows:

{% highlight javascript %}
function getSlope(data) {
    var sumProductXY = 0, sumProductX = 0, sumX = 0, sumY = 0;
    var n = data.length;

    for (var idx = 0; idx < n; idx++) {
        sumProductXY += idx * data[idx];
        sumProductX += idx * idx;
        sumX += idx;
        sumY += data[idx];
    }

    return ((n * sumProductXY) - (sumX * sumY)) / ((n * sumProductX) - (sumX * sumX));
}
{% endhighlight %}

What this function does is take an ordered array of numbers as the data parameter, loops through each element in the array to calculate the sumProductXY, sumProductX, sumX and sumY and then plugs those values into the equation for calculating the slope and returns the results. 

This is simplified a bit in that I'm making assumptions about our x-axis, which is a series of years, so I'm just summing our index in each loop iteration. This works well enough for our use case.

We can use this function to retrieve the slope of our base data set by grabbing the ticketsSold properties for each year, placing them in an array in order and calling it like so:

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

var ticketsSold = [];
for(var idx = 0; idx < ticketSalesPerYear.length; idx++) {
    ticketsSold.push(ticketSalesPerYear[idx].ticketsSold);
}

var slope = getSlope(ticketsSold);
{% endhighlight %}

<br />

# Calculating the y-intercept: $$ b $$

Now that we can calculate the slope for our data we need to calculate the y-intercept. The equation for calculating the y-intercept of our trendline is as follows:

<div class="eq-wrapper">
$$ b = [∑y - (m)(∑x)] / n $$
</div>

Getting to the code, the corresponding javascript implemention is:

{% highlight javascript %}
function getYIntercept(data) {
    var sumX = 0, sumY = 0;
    var n = data.length

    for (var idx = 0; idx < n; idx++) {
        sumX += idx + 1;
        sumY += data[idx];
    }

    var result = getSlope(data) * sumX;

    return (sumY - result) / n;
}
{% endhighlight %}

This function is pretty straight forward. Like we did when calculating the slope we loop through each element of our data array to calculate the sumX and the sumY. We then get the slope for the data using our getSlope() method and then plug that information into the equation and return the result. 

We can call this function the same we did for our getSlope() function like so:

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

var ticketsSold = [];
for(var idx = 0; idx < ticketSalesPerYear.length; idx++) {
    ticketsSold.push(ticketSalesPerYear[idx].ticketsSold);
}

var yIntercept = getYIntercept(ticketsSold);
{% endhighlight %}

<br />

# Solving for $$ y $$

OK, we have our $$ x $$ values, we have our $$ m $$ values and we even have our $$ b $$ values! So let's go ahead and get our $$ y $$ values that will use with their corresponding x-coordinates to plot our trendline. Again our equation for $$ y $$ is:

$$ y = mx + b $$

We can calculating our y-coordinate for each corresponding x-coordinate using a function like so:

{% highlight javascript %}
function getYCoordinate(data, axisX) {
    return (axisX * getSlope(data)) + getYIntercept(data);
}
{% endhighlight %}

This function takes our ordered data array as a parameter along with a x-axis/x-coordinate value and returns the corresponding y-axis/y-coordinate value using the functions we defined earlier and plugging those values into the equation.

# Putting it all together!

OK, we have everything we need. Now we just need to put it all together and retrieve the y-coordinates for year 1 and 11 and then add them as a series to the chart. The full example is as follows:

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
    <title></title>
<link rel="stylesheet" href="https://cdn.kendostatic.com/2015.1.318/styles/kendo.common-material.min.css" />
    <link rel="stylesheet" href="https://cdn.kendostatic.com/2015.1.318/styles/kendo.materialblack.min.css" />
    <link rel="stylesheet" href="https://cdn.kendostatic.com/2015.1.318/styles/kendo.dataviz.min.css" />
    <link rel="stylesheet" href="https://cdn.kendostatic.com/2015.1.318/styles/kendo.dataviz.materialblack.min.css" />

    <script src="https://cdn.kendostatic.com/2015.1.318/js/jquery.min.js"></script>
    <script src="https://cdn.kendostatic.com/2015.1.318/js/kendo.all.min.js"></script>
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

        var ticketsSold = [];
        for(var idx = 0; idx < ticketSalesPerYear.length; idx++) {
            ticketsSold.push(ticketSalesPerYear[idx].ticketsSold);
        }

        ticketSalesPerYear[0].trendline = getYCoordinate(ticketsSold, 1);
        ticketSalesPerYear[10].trendline = getYCoordinate(ticketsSold, 11);

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
                series: [
                    { field: "ticketsSold"},
                    { field: "trendline"}
                ],
                valueAxis: {
                  min: 1200000000,
                  labels: {
                    format: "n0" 
                  }
                },
                categoryAxis: {
                    field: "year",
                    majorGridLines: {
                        visible: false
                    },
                },
                theme: "materialblack"
            });
        }

        jQuery(document).ready(createChart);
        jQuery(document).bind("kendo:skinChange", createChart);

        function getSlope(data) {
            var sumProductXY = 0, sumProductX = 0, sumX = 0, sumY = 0;
            var n = data.length;

            for (var idx = 0; idx < n; idx++) {
                sumProductXY += idx * data[idx];
                sumProductX += idx * idx;
                sumX += idx;
                sumY += data[idx];
            }

            return ((n * sumProductXY) - (sumX * sumY)) / ((n * sumProductX) - (sumX * sumX));
        }

        function getYIntercept(data) {
            var sumX = 0, sumY = 0;
            var n = data.length

            for (var idx = 0; idx < n; idx++) {
                sumX += idx + 1;
                sumY += data[idx];
            }

            var result = getSlope(data) * sumX;

            return (sumY - result) / n;
        }

        function getYCoordinate(data, axisX) {
            return (axisX * getSlope(data)) + getYIntercept(data);
        }
    </script>
</div>


</body>
</html>

{% endhighlight %}

You can see I've added our new functions. In addition to that, right after declaring the source data array I update the first and last element of the array with a new property called "trendline" which is set to the value returned by the getYCoordinate call for the given year or x-coordinate. Finally, I added the "trendline" field as a new series in the series node of the chart configuration.

That's all there is to it. Once you have your y-coordinates all you have to do is add them to your data source as a new series and the chart widget takes care of drawing the trendline. Neat!

# Conclusion

I enjoyed solving this problem for myself but I would have appreciated it if the Kendo Chart had the ability to do this for me. If you agree, Telerik has a [feedback thread](http://kendoui-feedback.telerik.com/forums/127393-telerik-kendo-ui-feedback/suggestions/4966833-add-trendline-to-a-line-chart) for exactly that. Go vote so I can be lazier the next time some requests this feature! 







