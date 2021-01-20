---
title: "Using R to Explore The Impact of COVID-19 on Venice's Airbnb Rates"
author: "Matt Purtill"
date: "20/01/2021"
output:
   md_document:
      preserve_yaml: TRUE
always_allow_html: true

---

Prior to 2020 Venice was used as one of the most harmful examples of
overtourism in the world. Like all tourist hotspots, the impact of
short-term rentals on the local housing market has been under scrutiny,
with the threat of regulation over Airbnb-style home rentals.

The coronavirus pandemic changed everything, as international tourism
during the 2020. This article will examine the impact of the pandemic on
Venice’s Airbnb ecosystem between August 2019 and August 2020. We will
consider how the distribution and cost of properties changed in the
year, and which locations have bee n the most impacted.

Our Data Source
---------------

Data for this analysis is obtained from [Inside
Airbnb](http://insideairbnb.com/). This site offers accessible downloads
on Airbnb property data from major cities around the world. This gives
us in-depth data on each Airbnb property in Venice at the time of the
data scrape. Although it does not impact the data, it is worth noting
that the site takes a critical stance towards Airbnb’s impact on cities,
and provides its own analysis on featured cities.

This analysis will be constructed using R, using mostly tidyverse ggplot
visualisations and the leaflet mapping package. and an R Markdown copy
of the code is available here.

Venice’s Pre-Coronavirus Airbnb Ecosystem
-----------------------------------------

In August 2019 there are 8,907 properties listed for Venice with an ADR
of $139. However, we can see a massive range in average rates - from a
low of $9 a night to a suspicious value of $12,345. We can build a
histogram to display the spread of ADR across properties.

![](venice_airbnb_files/figure-markdown_strict/ADR%20histogram-1.png)

Lots of the definition in the low range is lost because of the few very
high ADR properties. As most of the action is in the $1000 or below
range, we can do an alternative histogram for properties that are in the
price range of us mere-mortals. As we are looking at a price variable, I
will also compare the distribution with a logarithmic value to see if it
brings to a more symmetical distribution.

![](venice_airbnb_files/figure-markdown_strict/ADR%20histogram%20elaborated-1.png)![](venice_airbnb_files/figure-markdown_strict/ADR%20histogram%20elaborated-2.png)

As we might expect, the ADR is distributed witsh a positive skew, which
becomes more symmetrical with a logarithmic function. This will be
useful to know for modelling later on.

Of course, not all properties are equal. The city of Venice includes the
well-known central islands, but also includes some more peripheral
islands, as well as a large area of the mainland, which connects to the
islands by bridge. We can plot the median daily rate for each
neighbourhood of the city to show how much of a premium is placed on the
a position in the islands. Unsurprisingly, the island neighbourhoods
also show the largest number of properties on short-term rental sites.

    ## OGR data source with driver: GeoJSON 
    ## Source: "/Users/mattpurtill/Desktop/Projects/venice_airbnb/neighbourhoods.geojson", layer: "neighbourhoods"
    ## with 118 features
    ## It has 2 fields

    ## Warning: Column `neighbourhood` joining factors with different levels, coercing
    ## to character vector

<iframe src="neighbourhood_map.html" height="600px" width="100%" style="border:none;">
</iframe>
Impact of COVID-19 on Summer Bookings
-------------------------------------

-   stays/availability
-   impact on prices on like-for-like properties

<!-- -->

    ## # A tibble: 1 x 4
    ##   properties_2019 properties_2020 price_2019 price_2020
    ##             <int>           <int>      <dbl>      <dbl>
    ## 1            8907            8399        110         93

There are approximately 400 fewer properties available in Venice, and
median daily rates are down by $17 (-15%).

![](venice_airbnb_files/figure-markdown_strict/comparing%20neighbourhoods-1.png)

Looking at the regional difference in rates and properties and the
difference between the islands (‘Isole’) and the mainland
(‘Terrafirma’). We can see that the largest impact was felt in the
islands. The largest reduction in rates came from the most expensive
regions of San Marco, Cannaregio and Castello. There is almost a
discrete difference in the rate changes for regions in the islands and
the regions on the mainland. We may expect that the mainland regions are
less impacted by rates because of their budget base prices. Other
economic activity, other than leisure tourism, may also have provided a
more stable customer base for mainland rentals.

Lido is the one island region with an increase in rates year-on-year.
Lido may have fared well year-on-year because of its position as the
city’s summer beach resort on the edge of the lagoon. The region may
have been boosted by increased numbers of domestic breaks during the
pandemic. Additionally, the increase in rates may be due to slightly
changing dates from when the data was scraped - the 2019 rates are from
early August, whereas the 2020 rates were scraped towards late August
just ahead of the popular Ferragosto holiday, when these beach
properties would be at their highest demand.

![](venice_airbnb_files/figure-markdown_strict/direct%20impact%20on%20same%20listings-1.png)

We can also track individual properties that have been listed over the
past year and use a boxplot to compare the difference in rates between
the islands and the mainland. We can also take into account the type of
property - whether it is a whole property or a private room in a larger
building, which we would expect to have an impact on rate dynamics.

In the islands there is a similar average reduction in rate across both
types of property. In these properties, the median reduction in rates is
12%. However, on the mainland it is only in private rooms where there
has been a median reduction rate year-on-year. Among entire properties
on the mainland, median rates have remained the same despite the
coronavirus pandemic. It is worth commenting that in all groups there is
a wide range of rate changes, partly due to the individual circumstances
of each property, but is perhaps an indication that there is no single
strategy that Airbnb hosts has taken during the pandemic.

<iframe src="property_map.html" height="600px" width="100%" style="border:none;">
</iframe>
Using R’s leaflet mappping package, we can dive deeper into the data,
plotting each property that appears in 2019 and 2020. The colour of each
dot represents the change in daily rate on a year-on-year basis.
Focussing on just the historic main islands, there are far more reds
than blues, showing how rates have tumbled in the historic centre of the
city - although even in these locations there are still a number of
properties that have raised their rates.

We might expect that central areas close to tourist attractions are the
most impacted by the reduction in the number of visiting tourists.
Around these areas there are plenty of properties that cut their rates,
but also a fair number of properties that have raised their rates from
2019. This is surprising, and is maybe something to investigate with
further research or modelling.

Also of interest is the reduction in rates in the south-east of the main
islands. This drop in rates appears to be consistently steep across most
properties in this area. As well as the coronavirus pandemic, one
additional difference between 2019 and 2020 is that the previous year
saw the Venezia Biennale take place at the nearby Giardini della
Biennale. With this popular arts event not taking place in 2020, this is
another reason why nearby rates may have decreased so much. More
broadly, this may be an additional contributing reason why rental rates
would be lower in 2020 in the absence of COVID.

Conclusions and Further Analyses
--------------------------------

With this analysis we have begun to dig into some of the drivers for
Airbnb rents in Venice. We have been able to confirm some prior
assumptions that rates are higher in the historic centre of the city,
and we have also seen how different regions of the city have been
impacted separately from the coronavirus pandemic. In our final mapping
of properties with annual rate changes, we have been able to identify
some overall trends on rate based on property location. However, it is
clear that there is much to rate changes than location. This analysis
has only scraped the surface of what is available in this dataset so
there is a large scope for future analysis:

-   Using quantitative measures and text descriptions to model rates and
    rate changes
-   Track the impact of amenities and descriptions on property review
    scores
-   Compare the difference in the market for hosts with multiple
    properties compared to small-scale single unit hosts
