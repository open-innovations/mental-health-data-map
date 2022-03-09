# mental-health-data-map

> This is a repository / visualisation pulled together as part of the Open Data Saves Lives [innovation working session held at Open Innvoations on 7th and 8th March 2021][EVENT_1].

**TL;DR** There is a [visualisation][VIZ] if you just want to explore the data.

## Overview

This is an exploration and visualisation of some Mental Health Datasets, along with demographic information, such as ethnic origin and population.
In principle, this could be expanded to include any information that could be mapped to Clinical Consulting Group.

## Datasets

### Community Mental Health Survey

The [CMHS (Community Mental Health Survey)][CMHS_PAGE] is an annual survey which looks at the experiences of people who use community mental health services.

The survey covers the period from September to November 2021 (which defines the timing of the rest of the mental health data).

The [cmhs.ipynb][CMHS_JUPYTER] workbook contains code to download an process this data from source into the required format.
Processing comprises concatenating the Trust Respondents and Trust Scores files before summmarising at CCG level and dropping some columns.
Only mean scores are presented, so the aggregation is a the `mean` function.

### Out of Area Placements in Mental Health Services

Out of area placements (OAPs) in acute inpatient mental health services in England from NHS and independent providers.
This can be found on the [Out of Area Placements in Mental Health Services page][OAP_PAGE] on the NHS Digital site.

The data presented is taken from the November 2020 statistics. The Open Data is linked from the specific sub-page for November 2020.
Data is in a long form, broken down with questions and values broken down by a series of Breakdowns.

Only the following questions are included:

* Inappropriate OAPs active at period end
* Total Cost For Inappropriate OAPs in Period

Values are selected from the `ThreeMonth` grouping (running from 01/09/2020 to 30/11/2020) and the `Ccg` Breakdown.
The CCG Code is assumed to be a `CCG21CDH` code, although this may be an unsafe assumption. Attempting to use the CCG20 series mappings leads to more errors.

The [oaps.ipynb][OAP_JUPYTER] workbook contains code to download an process this data from source into the required format.

### Mental Health Services Dataset

The [Mental Health Services Dataset (MHSDS)][MHSDS_PAGE] brings together information captured on clinical systems as part of patient care. It covers not only services provided in hospitals but also outpatient clinics and in the community, where the majority of people in contact with these services are treated.

It is not possible to re-run the generation of this data without the processed data provided by the DHSC team that attended the hack day,
although it looks like it is derived from [MHSDS Monthly Reports][MHSDS_MONTHLY_PAGE].

The [mhsds.ipynb][MHSDS_JUPYTER] assumes that the pre-formatted file exists in the `source` directory.
It then cleans up the data, summarises by average measurement per CCG (assumed to be CCG21 series coding).
The measures presented are related to restraint.

### Demographic data

Two sets of data are also included

* Origin data (via Marc Farr)
* [ONS population estimates for CCG21][ONS_CCG_POPULATION]

[ONS_CCG_POPULATION]: https://www.ons.gov.uk/peoplepopulationandcommunity/populationandmigration/populationestimates/datasets/clinicalcommissioninggroupmidyearpopulationestimates

These are included in the [data-wrangling][DATA_WRANGLING] workbook.

## Reference Data

Some of the reference data is retrieved from source in the [lookup-codes][LOOKUP_CODES_WB] workbook.
GeoJSON files are downloaded and processed (simplified) in the [geojson][GEOJSON_WB] workbook.

[LOOKUP_CODES_WB]: lookup-codes.ipynb
[GEOJSON_WB]: geojson.ipynb

### CCG Codes

There are two series of CCG codes: a short version and a longer version. These are referred to by date:

* `CCGyyCD` - long ONS style code commencing with `E38`
* `CCGyyCDH`

It is not always clear which short code is being used, meaning some data loss has occured in processing the data.

### GeoJSON layers

Two GeoJSON layers are checked in to the repository, representing the CCG20 and CCG21 boundaries.

## Issues

* CCG codes are not always referenced to a year, so significant guesswork is required.
* Reliable lookups between Trust and CCG are not available, particularly for historical data.
* Some Trusts are commissioned by groups of CCG meaning there is a one to many mapping from trust to CCG. This makes aggregation problematic.

## Running the code examples locally

All the example workbook code can be run locally, although not all have the required source data.

You can use docker-compose to start a local Jupyter lab. You'll need to set up [docker][DOCKER]
and [docker-compose][DOCKER_COMPOSE] for yor platform.
The [Jupyter Docker images][JUPYTER_DOCKER] documentation will be helpful.

Of course, you could also use the code in a hosted envioronment such as [Google Colab][COLAB].


[EVENT_1]: https://opendatasaveslives.org/events/bNNM2sOfPlwv
[EVENT_2]: https://opendatasaveslives.org/events/2U5xyloGlvNTC
[VIZ]: https://open-innovations.github.io/mental-health-data-map/


[CMHS_PAGE]: https://www.cqc.org.uk/publications/surveys/community-mental-health-survey-2021
[CMHS_JUPYTER]: chms.ipynb

[OAP_PAGE]: https://digital.nhs.uk/data-and-information/publications/statistical/out-of-area-placements-in-mental-health-services
[OAP_JUPYTER]: chms.ipynb

[MHSDS_PAGE]: https://digital.nhs.uk/data-and-information/data-collections-and-data-sets/data-sets/mental-health-services-data-set
[MHSDS_MONTHLY_PAGE]: https://digital.nhs.uk/data-and-information/publications/statistical/mental-health-services-monthly-statistics
[MHSDS_JUPYTER]: mhsds.ipynb

[DATA_WRANGLING]: data-wrangling.ipynb

[DOCKER]: https://docs.docker.com/get-docker/
[DOCKER_COMPOSE]: https://docs.docker.com/compose/install/
[JUPYTER_DOCKER]: https://github.com/jupyter/docker-stacks
[COLAB]: https://colab.research.google.com/gitpo