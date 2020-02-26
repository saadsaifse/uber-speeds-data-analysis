# Uber Speeds Data Analysis

Ride-hailing has become an increasingly common mode of transportation in every urban city of the world. With the convenience of smartphones, 4G data, and secure payment systems – ride-hailing turns out to be a natural and much needed facility defying all traditional transport mechanisms of the past. 

In 2009,  Uber was launched,  which was the first of its kind company that started offering ride-hailing services in select places of the world. Today Uber is available in 65 countries and over 600 countries worldwide. [Uber completes 14 million trips each day](https://www.businessofapps.com/data/uber-statistics/). Another way of looking at Uber is through a lens of technical capabilities that it has to offer.  Today, it is considered among the top tech companies in the world.  As more and more people use Uber, it gains massive amount of movement and speeds data of the vehicles, that is also publicly available for scientific research and urban planning purposes. One such dataset,  that was released on `May 14th,  2019`,  is the **Movement Speeds** of Uber vehicles recorded around the world.

In this project, Movement Speeds data for Uber rides in `London` city are analyzed. Hourly time series of speeds data is decomposed and predicted during the `1st` quarter of 2018. Furthermore, road accidents (collisions) data from 2018 is read in hope to be correlated with the speeds dataset. 

The objectives of this project are

1. Analyze hourly speed time series to find patterns and relationships.
1. Predict per road future speeds based on the current time-series
1. Analyze and correlate collision data with speeds dataset, to answer questions such as, when did most collisions occur? 

To encourage further development, complete source code and analysis is available in the [src](./src/fullAnalysis.ipynb) directory. This project is open to contributions.

## Implementation

The implementation of the project is divided into three sections. First, the data collection
and deciding what kind of information is available out there that can be related with the
datasets. Second, the preparation of data, that includes, data cleaning, sorting, merging
and filtering meaningful information on which analysis can be performed.
Finally, time-series decomposition, forecasting, and modeling to find patterns, relationships, and predictions from the dataset.

### Data Collection
To perform meaningful analysis, London city was selected as it has higher percentage
of Uber rides with respect to other countries in the region. There are two different kinds
of datasets offered by Uber.

1. **Hourly Time Series** – Provides hourly mean of speeds and their standard deviations across the city for historic data
1. **Quarterly Statistics** by Hour of Day – This dataset provides the average, standard
deviation, 50th percentile, and 85th percentile speeds aggregated by hour of day
across all days in the specified quarter.

#### Selecting Duration of Data
In this project, we only take the Hourly Time Series dataset for time series analysis of
speeds data. Since this is an hourly time series, a month of data could have `730,001`
mean speed observations. For this reason, only three months of hourly time series data
is collected and analyzed.

#### Selecting Interval of Data
The next question is for what time the data should be collected? Since it is available for
`2018`, `2019`, and so on. The decision to choose the time depended on collision dataset for
the London city during that time. Both collision data and speed datasets has to be for
the same time interval to be able to form a meaningful correlation between the two.

Looking up at [UK government data website](https://data.gov.uk/), it was found that Road Safety Accidents
dataset is available for [2018](https://data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data/datafile/36f1658e-b709-47e7-9f56-cca7aefeb8fe/preview) and it was decided to select 2018 data for both datasets.

#### Downloading Datasets

Uber provides a handy [Movement Data Toolkit](https://www.npmjs.com/package/movement-data-toolkit) that is used to download the `.csv` files
for speeds dataset and is available as an `npm` and `Node.js` package.

Using the `speeds-transform` command of the toolkit, three months speed data for London can be downloaded. This data comes in `.csv` file. 

```bash
mdt speeds-transform historical London 2018-01-01 2018-03-31 --output=speeds-data.csv
```

Since it is alot of data, you can maximize the memory consumption of Node.js by prepending node options to the above command.

```bash
export NODE_OPTIONS="--max_old_space_size=8192" && mdt speeds-transform historical London 2018-01-01 2018-03-31 --output=speeds-data.csv
```

Whereas the road safety accidents collision data is a single file that can be downloaded
[here](https://data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data/datafile/36f1658e-b709-47e7-9f56-cca7aefeb8fe/preview)