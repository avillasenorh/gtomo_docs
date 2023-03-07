# Data preparation

## Station file

The first step in data processing is to obtain the station information (station code, coordinates, and elevation).
The latest EHB datasets (EHB08 and ISC-EHB) also include a station file
named `stations_ehb`, with station codes and coordinates.
The most relevant fields are:

- station code
- geographic latitude (degrees) in the range [-90, 90]
- longitude (degrees) in the range [0, 360]
- elevation (meters above sea level)

The station file contains a 2-line header, and a 5-line footer.
After each line containing station information there are two
extra blank lines:

```
Code   Station name/Region                                 Latitude  Longitude Elevation   Depth Prime  Status
==============================================================================================================
109C                                                        32.88925  242.89000       0.0                Open


115A                                                        32.69972  247.77000       1.0                Open


116A                                                        32.55933  248.30000       0.0                Open


A04A                                                        48.71890  237.29000       0.0                Open


A05A                                                        48.99965  237.91000       0.0                Open


A06A                                                        49.09756  238.52000       1.0                Open

...

ZUL                                                         47.48077    8.39000     700.0                Open


ZUR                                                         47.36783    8.58000     604.0                Open


ZZT                                                         35.36345  270.62800     120.0                Open


International Seismological Centre
Pipers Lane, Thatcham, Berkshire
United Kingdom RG19 4NS
voice: +44 (0)1635 861022
fax: +44 (0)1635 872351
```
Station elevations should be checked against the latest version of the ISC 
[station registry](http://www.isc.ac.uk/registries/) and/or a moder digital elevation model.
For example in the registry the station elevation for station A06A (Chilliwack, BC, Canada)
is 576 m, while in the above file is 1 m.

## Get station info from RES files

Station coordinates and elevations can also be obtained from the `RES` files, together
with additional information, such as the number of phases and the dates of the first and
last reported phase. It is preferable to obtain the station information from the `RES` file
because they are the values that were actually used for location.

### Geocentric vs geographic

**IMPORTANT**: Event and station coordinates in the `RES` files are given in geocentric 
coordinates not in the usual geographic (also called geodetic) coordinates.
In addition, colatitude is reported instead of latitude in the `RES` files.
In the EHB location code and in the tomography codes
the coordinates used are geocentric (co)latitude and longitude because they are more
convenient to calculate distances on an ellipse. Since we assume that the Earth is
an ellipsoid of revolution, geocentric and geographic longitude are identical. However
geographic and geocentric latitudes differ.

If \( \varphi \) is the geographic latitude and \( \varphi^\prime \) is the geocentric latitude, then

$$\tan \varphi^\prime = (1-e^2) \tan \varphi$$

where \( e \) is the [eccentricity](https://en.wikipedia.org/wiki/Eccentricity_(mathematics)).

Isolating \( \varphi^\prime \)

$$\varphi^\prime = \arctan [(1-e^2) \tan(\varphi)]$$

The relationship between eccentricity \( e \) and flattening \( f \) is 
\( e = \sqrt{ f ( 2 - f) } \), where flattening is defined as \(f = \frac{a-b}{a} \).

The geocentric latitude is consequently lesser (in absolute value) than the geographic
latitude, except at the equator and the poles where they are equal.
See this [link](https://proj.org/operations/conversions/geoc.html).

![geocentric_latitude](geocentric_latitude.png)

**IMPORTANT:** An additional complication is that the tomography code uses co-latitude \( \theta \)
instead of latitude \( \varphi \). The relationship between both is:

$$\theta = 90^\circ - \varphi$$

### Codes

The first code used to extract the station information from a RES file is `readsta`.

This code reads an input file called `res.in` that contains the list of `RES` files to be
processed. For example, for the EHB02 dataset:

```
64-69RES
70-75RES
76-81RES
82-87RES
88-93RES
94-97RES
98-00RES
```

The function `readres` defined in `readres.f` can read `RES` files for the EHB02, EHB03, and EHB04 dataset.
It must be modified to read the format of the more recent EHB08 and ISC-EHB datasets.

The output of `readsta` is a file called `ehb.sta` with the following format:

```
DAR      102.327   130.818  0.006      9193  19640101  19720604
PMG       99.344   147.159  0.065     45042  19640101  20001231
CTA      109.965   146.254  0.358     80830  19640101  20001231
RAB       94.165   152.163  0.185     23890  19640101  20001231
MAN       75.434   121.078  0.070      8975  19640101  19910424
MUN      121.805   116.208  0.253     26967  19640101  20001230
ADE      124.786   138.709  0.655     23056  19640101  20001231
BRS      117.234   152.775  0.525     52981  19640101  19931201
...
PINOR     46.382   239.128  1.510        44  20001115  20001231
BRNJ      49.508   285.434  0.050        22  20001116  20001222
SLFW      42.434   239.472  1.750         3  20001123  20001125
BDM       52.233   238.134  0.220        25  20001202  20001230
KNZ      128.832   177.674  0.218        23  20001210  20001231
LAMM      48.582   237.374  1.769         1  20001220  20001220
PCMD      43.304   237.700  0.239         2  20001224  20001231
```

The meaning of the columns is the following:

- station code
- geocentric colatitude in degrees, range: [0, 180]
- geocentric longitude in degrees, range: [0, 360]
- elevation in km
- total number of phases for each station
- date of first reported phase: YYYYMMDD
- date of last reported phase: YYYYMMDD

If a station in the `RES` files has the same station code but different coordinates it is treated
as a new station. Therefore `ehb.sta` can contain duplicated stations. To find out the station
codes with more than one entry:

    $ awk '{print $1}' ehb.sta | sort | uniq -cd       # guniq in macOS

It is more convenient to have the station file sorted by station name. Therefore
we create a new file `sort.sta`:

    $ sort +0 -1 +5n -6n -o sort.sta ehb.sta   # this command can be simplified and in outdated syntax

Since the coordinates in `sort.sta` are geocentric, they need to be converted to geographic in
order to plot the station on the map, and in case we want to compare them with the coordinates in 
`stations_ehb`:

```console
sta2geo << END
sort.sta
END
```

Output file is `ehbgeo.sta` (need to check this!!!)

Finally the sorted station file needs to be converted to the station file format used by the
tomography codes. This is carried out by other small program called `checksta` (does some
additional checks such as duplicated stations????)

```console
checksta << END
sort.sta
```

This produces and output file with the name `num_stat_list` and the following format:

```
A11          1  -70.198   47.050    0.061
A16          2  -70.006   47.278    0.015
A21          3  -69.690   47.511    0.046
A54          4  -70.413   47.264    0.381
A61          5  -70.090   47.501    0.358
A64          6  -69.892   47.634    0.137
AAA          7   76.947   43.079    0.800
AAE          8   38.766    8.969    2.442
AAHD         9   32.753   23.604    0.180
...
ZSC       6750  121.186   30.926    0.090
ZSP       6751 -122.257   37.758    0.119
ZST       6752   17.103   48.004    0.250
ZUG       6753   41.883   42.324    0.110
ZUL       6754    8.390   47.289    0.740
ZUR       6755    8.580   47.176    0.604
ZZT       6756  -89.372   35.182    0.120
```

The meaning of the columns is the following:

- station code
- station number
- geocentric longitude in degrees, range [-180, 180] (check!!!)
- geocentric latitude in degrees, range [-90, 90]  (check!!!)
- elevation in km

[Check `checksta` to see if conversion to latitude and longitude is correct)

All this process has been combined in a single shell script `stalist.csh`,
located in `/Volumes/DATA4/EHB/data/EHB03/STATIONS/stalist.csh`

[This script should be re-written in bash shell and lots of unnecessary checks removed]

```
#!/bin/csh
set nonomatch

# Cleanup previous run
/bin/rm -f sort.sta num_stat_list duplic.sta duplic.list

# Create station list from RES file (ehb.sta)
readsta

# Sort ehb.sta by station name (1st field) and by
# date of first reading (6th field)

sort +0 -1 +5n -6n -o sort.sta ehb.sta
wc -l sort.sta

# First create output in geographic lat,lon coordinates for plotting
sta2geo << END
sort.sta
END

# Then check for duplicated stations

awk '{print $1}' sort.sta >! sta.m             # ".m" stands for multiple
awk '{print $1}' sort.sta | sort -u >! sta.u   # ".u" stands for unique
comm -23 sta.m sta.u >! sta.d

sort -u sta.d >! tmp
diff sta.d tmp
wc -l sta.d tmp
/bin/rm -f tmp

set nd = `cat sta.d | wc -l`

@ i = 1
touch duplic.list
while ($i <= $nd)

    set sta = `awk 'NR == i' i=$i sta.d`

    awk '$1 == kstnm' kstnm=$sta sort.sta >> duplic.list
    echo "" >> duplic.list

    @ i++

end

# run checksta to create num_stat_list

checksta << END
sort.sta
END


wc -l num_stat_list duplic.sta
/bin/rm -f sort.sta sta.m sta.u sta.d
```

Station file is read by `trres` with the following format:

```fortran
      read(30,3002,end=100,iostat=ios) ksta,idum,rlon,rlat,relev
 3002 format(a6,i8,3f9.3)
```
