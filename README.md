# Earthquake risk embedded in property prices: Evidence from five Japanese cities
### Masako Ikefuji, Roger J. A. Laeven, Jan R. Magnus & Yuan Yue
# Author Contributions Checklist Form

## 1 Data

### Abstract

Data used in this paper comprises information collected from various sources. Our main data source
is the Real Estate Transaction-Price Information provided by the Ministry of Land, Infrastructure,
Transport and Tourism (MLIT) of Japan. Other data sets include: (i) macroeconomic indicators
provided by the Japan Cabinet Office, (ii) ward characteristics provided by the portal site of official
statistics of Japan, and (iii) long term earthquake probabilities provided by the Japan Seismic Hazard
Information System. We have simulated short term earthquake probabilities using an Epidemic Type
Aftershock Sequence (ETAS) model which we have estimated using historical earthquakes provided by
the Japan Meteorological Agency. The property location and earthquake risk information have been
linked using the Japanese meshcode system and the geocoding service provided by Google Maps.

### Availability

All data used in this paper has been generated from publicly available sources. The cleaned and
compiled data sets can be found athttps://github.com/yy112/earthquake-riskunder the folder
‘data’. (Source data files can be provided upon request.)

### Description

The data can be briefly described as follows—our online Data Documentation that is available from
https://bit.ly/3qHcTQ3provides many more details.

Our data set contains four separatecsvfiles:

```
1.individualdata.csv
```
Data from various sources have been cleaned and compiled into the data fileindividualdata.csv
(size: 214.7 MB). This file is compressed in thezipfile:individualdata.zip(size: 11.4 MB). Each
row in this file represents one property transaction record, with information on the characteristics


of the transaction, characteristics of the property, earthquake probabilities associated with the area
that the property is located in, macroeconomic variables observed at the transaction period, and
demographic characteristics of the ward that the property is located in. There are also interaction
terms and derived variables.

```
2.Xpsi-1.csv
```
Short run earthquake risk data obtained through simulation has been stored inXpsi-1.csv(size:
2KB). This file contains five time series (quarterly frequency) of the short run probabilities, each
column corresponding to one of the five cities included in the scope of our analysis.

```
3.cityrange.csv
```
The filecityrange.csv(size: 1KB) contains the chosen spatial window (for the estimation of the
ETAS models) corresponding to each city. Each row represents one city and the variables used in our
analysis are “latMin”, “latMax”, “lngMin”, “lngMax”, corresponding to the minimum and maximum
values of the latitude and longitude of the chosen spatial window (we have chosen to use rectangular
spatial windows).

```
4.JMArecords.csv
```
Earthquake records collected by the Japan Meteorological Agency (JMA) have been stored in
JMArecords.csv(size: 24.15 MB). Each row of this file contains one earthquake record, with infor-
mation on the time and location of the earthquake. A detailed description of the records can be found
inhttps://www.data.jma.go.jp/svd/eqev/data/bulletin/data/shindo/format_e.txt.

The data sets have been collected from the sources described below:

We use the real estate transaction price and property characteristics information provided by the
Ministry of Land, Infrastructure, Transport and Tourism (MLIT):

```
https://www.land.mlit.go.jp/webland_english/servlet/MainServlet
```
```
GDP figures are provided by the Cabinet Office website:
```
```
https://www.esri.cao.go.jp/index-e.html
⇒https://www.esri.cao.go.jp/en/sna/menu.html(2018 August)
```
accessed on April 30, 2016.

CPI data are provided by the Statistics Bureau of Japan, Ministry of Internal Affairs and Com-
munications website:

```
https://www.stat.go.jp/english/data/cpi/index.html.
```
We use the version released on August 26, 2016, which is 2015-based and integrate the monthly data
into quarterly data using simple averages.

```
Ward attractiveness characteristics are provided by the Portal Site of Official Statistics of Japan:
```
```
https://www.e-stat.go.jp/.
```
```
The seismic intensity data are provided by the Japan Meteorological Agency (JMA):
```
```
https://www.data.jma.go.jp/svd/eqev/data/bulletin/shindo_e.html.
```
Long term earthquake risk (derived from the Probabilistic Seismic Hazard Maps) is provided by
The Japan Seismic Hazard Information Station (J-SHIS):


```
http://www.j-shis.bosai.go.jp/map/JSHIS2/download.html?lang=en.
```
Property location and risk information are linked by using the Geocoding services provided by
Google Maps:

```
https://developers.google.com/maps/documentation/geocoding/start.
```
The Cabinet Office Homepage, the Statistics Bureau, the Portal Site of Official Statistics of Japan,
and the Japan Meteorological Agency permit free usage of content provided on their website. Their
Terms of Use are compatible with the Creative Commons Attribution License 4.0. The J-SHIS permits
free downloading and usage of data for purposes that do not violate any law.

## 2 Code

### Abstract

The code for our analysis has been written in programming languageRand consists of two major
parts. (i) The first part concerns the estimation and simulation of the temporal ETAS models. The
functions used in this part are collected inetasfuncs.R. (ii) The second part concerns the estimation
of the multivariate error components model. The functions used in this part are collected in anR
packagemvecr. The manual of this package (mvecrmanual.pdf) provides detailed descriptions about
the inputs and outputs of each function in this package. Finally, detailed code and instructions to
replicate the tables and figures in this paper can be found inreplicationinstructions.Rmd/pdf.

### Availability

The fullRcode for this paper (i.e.,etasfuncs.R, theRpackagemvecr, and the replication master
filereplicationinstructions.Rmd/pdf) is available from our GitHub repositoryhttps://github.
com/yy112/earthquake-risk.

### Description

replicationinstructions.Rmdandreplicationinstructions.pdfcontain the annotated code
and instructions to replicate the tables and figures in the paper. The code consists of two parts (that
can be executed independently):

A. Estimation and simulation of the ETAS model(etasfuncs.R)

- Estimation of the ETAS model, replication of the summary statistics and estimation results in
    Tables 48 and 50 of the Data Documentation. Computation time needed: 5 minutes.
- Simulation of short run earthquake probabilities, using the estimated ETAS parameters and a
    historical earthquake catalog provided by the Japan Meteorological Agency. Computation time
    needed: around 24 hours for 30,000 Monte Carlo runs for each city. The output of the simulation
    is contained in Xpsi-1.csv. Figures 1 and 2 of the paper can be replicated using the data
    provided inXpsi-1.csvwithout having to perform the simulation first.

The following functions contained inetasfuncs.Rare used:

- EQcatalog: this function generates an earthquake catalog in the format that can be used for the
    estimation of the ETAS model;


- etasestim: this function estimates the parameters of the ETAS model using thePtProcess
    package;
- etasprob: this function calculates the earthquake probability forecasts within a given time
    period through simulation using thePtProcesspackage.
- genXpsicity: this function generates the simulated earthquake probabilities for each city for
    the entire sample period (2006Q2–2015Q3) usingetasprob.

B. Estimation of the multivariate error components regression model(Rpackagemvecr)

- Characteristics of the housing data set. Replication of Table 1 of the paper.
- Characteristics of the JSHIS long run probabilities. Replication of Table 2 of the paper.
- Main estimation results. Replication of Table 3 and Figure 3 of the paper. Computation time
    needed: 1–1.5 hours for each model (with a specified value forψ).
- Sensitivity analysis regarding probability weighting functions. Replication of Table 4 and Figure
    4 of the paper. Computation time needed: 1–1.5 hours for each model.
- Sensitivity analysis regarding other model specifications. Replication of Tables B1–B5 of the
    supplementary material. Computation time needed: 1–1.5 hours for each model with 2 error
    components, 3–4 hours for the 3 error components model, 40–60 minutes for the model where
    individual data is grouped by nearest station instead of by district.
- Importance ordering and decomposition of risk premia. Replication of Tables C6–C7 of the
    supplementary material. Computation time needed: 1 minute.

The following main functions contained inmvecrare used:

- vectorize: this function takes the individual records as input, groups them by the specified
    “time”, “district”, and “type” dimensions, takes the averages within each group, and stacks the
    results into vectors and matrices that can be used in the multiple error components regression;
- ecreg: this function takes the “vectorized” data and performs maximum likelihood estimation
    of the multivariate error components model;
- regpsi: this function takes a step further and allows one of the regressors to be transformed
    by a single-parameter function, with a given parameterψ;
- optpsi: this function is a wrapper function aroundregpsithat implements a grid search to
    find the optimalψ. Given a list of candidate parametersψ, it calls the functionregpsifor each
    value ofψon the list and records the loglikelihood value. The parameter corresponding to the
    highest value of the log likelihood is chosen asψˆ.

## 3 Instructions for Use

### Reproducibility scope

All tables and figures in the paper can be reproduced using the fourcsvdata sets and the annotated
Rcode and instructions contained inreplicationinstructions.Rmd/pdf(see also Section 2 above),
all of which is available fromhttps://github.com/yy112/earthquake-risk.


### Installation instructions and supporting software requirements

Rversion later than 3.6.0 is needed. Additionally the packagesdplyr(version 1.0.0),PtProcess(ver-
sion 3.3-13),readr,R.utils,zooandmvecrare needed. To install the packagemvecr, download the
source file (mvecr0.3.0.tar.gz) fromhttps://github.com/yy112/earthquake-riskand choose
“Install packages from Package Archive File (.zip; .tar.gz)” inR. More detailed installation instruc-
tions can be found inreplicationinstructions.Rmd/pdf.


