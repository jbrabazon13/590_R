Introduction
------------

Hydrographs give inference into groundwater inflow as well as storm and flood events for a given stream. Here I create a hydrograph to ultimately understand base-flow, major flood events, and storm events for the Obed River in Lancing, TN for the year 2016. Base-flow of a stream or river is the flow at which the major contribution of inflow is groundwater. Major flooding events can be distinguished by anomalously large peak-flow. Storm events are very similar to flooding events but are on a much smaller scale. Data to create this hydrograph was taken from a USGS monitoring site 03539800 on the Obed River (data taken from <https://nwis.waterdata.usgs.gov/nwis/uv?cb_00060=on&format=rdb&site_no=03539800.>).

``` r
library(ggplot2)
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
library(gridExtra)

#import OBED_data
df <- read.table("../data/OBED_data.txt", skip = 29)

#merge time and date together and make a dataframe of date.time and flow
date.time <- as.POSIXct(paste(df$V3, df$V4), format="%Y-%m-%d %H:%M")
flow <- df$V6
attr(flow, "units") <- "cfs"
hydrograph_data <- data.frame(date.time,flow)

#plot the hydrograph
ggplot(hydrograph_data, aes(date.time,flow))+
  geom_line()+
  ylab("Flow in cfs")+
  xlab("Date")
```

![](task6_notebook_files/figure-markdown_github/unnamed-chunk-1-1.png)

Figure 1 Hydrograph for the Obed River

``` r
#find the max flow, minimum, and average flow and put them in a table
max.flow <- max(hydrograph_data$flow)
min.flow <- min(hydrograph_data$flow)
avg.flow <- round(mean(hydrograph_data$flow), digits = 0)
hydrograph_stats <- data.frame(max.flow,min.flow,avg.flow)
print(hydrograph_stats)
```

    ##   max.flow min.flow avg.flow
    ## 1    36200     0.04      761

Table 1: Descriptive statistics of the Obed River

Discussion
----------

The hydrograph of the Obed River allows interpretations to be made of the base-flow, flood events, and storm events.

### Base-flow

The flow on the Obed River seems to be relatively low in August through the end of November. Flow slowly decreases from nearly 30cfs in August to around 1cfs in October and November. This period between August and November is a good estimate of base-flow as rain events seem to be seldom, or even absent. The base-flow of the Obed River could likely be estimated to be in the 1cfs to 30cfs range. The minimum flow of the river is 0.04cfs as shown in Table 1. This seems to be an anomaly in the data. Perhaps sensors were not working during this time. This measurement took place in December in between larger fluxes approaching 1,000cfs in flow.

### Flood Events

Flood events can be characterized by large peaks on the hydrograph. The maximum flow for 2016 was 36,200cfs as shown in Table 1. This is likely to be a flood event. This event occurred on February 16th, 2016 with its peak at 5:00 am. It is evidenced as the largest spike in the hydrograph. It is likely that an excessive rain event could have caused such a large surge in flow. More research is to be done to fully understand the cause of this event.

### Storm Events

Storm events are most likely characterized as mid-sized peaks on the hydrograph. As seen on the hydrograph, several smaller peaks are prevalant during spring and fall months. These are likely rain or storm events that had appreciable rainfalls that allowed run-off to increase the flow of the Obed River. These storm events seem to range in their respective peak flows from nearly 1,000cfs to around 10,000cfs.
