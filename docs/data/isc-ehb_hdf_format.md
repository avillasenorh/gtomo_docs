```
                  ISC-EHB (2000-2013) HDF FILE FORMAT

*HDF file read statement

       read(1,100) 
     1 ahyp,isol,iseq,iyr,mon,iday,ihr,min,sec,ad,glat,glon,
     2 depth,iscdep,mb,ms,mw,ntot,ntel,ndep,igreg,se,ser,sedep,
     3 rstadel,openaz1,openaz2,az1,flen1,az2,flen2,avh,ievt
 100  format(a1,a3,a2,i2,2i3,1x,2i3,f6.2,a1,2f8.3,2f6.1,3f4.1,
     1 4i4,3f8.2,3f6.1,4i4,f5.1,i10)

Variable definitions:

ahyp         secondary teleseismic azimuth gap       a1

             blank = <= 180 deg
             Z     = >  180 deg

isol         solution type                           a3

             HEQ = origin time & hypocenter fixed
             DEQ = depth free (sedep <= 15 km)
             FEQ = depth fixed by review
             LEQ = depth fixed by program (sedep > 15 km)
             XEQ = poor solution (ser > 35 km)

iseq(1:1)                                            a1

             M = GCMT solution available
             X = explosion

iseq(2:2)                                            a1

             d = depth constrained by depth phases
                 with sedep < 5 km and at least 3 
                 defining depth phases             
             b = depth set to USGS broadband depth             
             h = depth set to GCMT depth
             g = depth set to ISC-GEM depth provided 
                 there are no other constraints on 
                 depth
             o = depth set to 10 km for oceanic events 
                 provided there are no other constraints 
                 on depth
             c = depth set to 15 km for events within 
                 known regions of shallow seismicity 
                 provided there are no other constraints 
                 on depth            
             t = depth set to 15 km for events within 
                 a radius (distance and depth) of 55 km 
                 to a trench provided there are no other
                 constraints on depth
             v = depth set to 15 km for events within 
                 a radius (distance and depth) of 55 km 
                 to a volcano provided there are no other
                 constraints on depth
             s = depth from research studies
             i = depth set to ISC depth 
             x = depth reviewed but uncertain
                 DEQ x assigned to events with sedep 
                 > 5 and < 15 km
             n = depth set to nearest neighbor with a 
                 well constrained depth
             z = explosion or earthquake location known 
                 to z km where z is a number from 0-9

iyr          year                                    i2
mon          month                                   i3
iday         day                                     i3
ihr          origin hour                             i4
min          origin minute                           i3
sec          origin second                           f6.2
ad           source agency                           a1
glat         geographic latitude                     f8.3
glon         geographic longitude                    f8.3
depth        depth                                   f6.1
iscdep       ISC depth                               f6.1
mb           mb magnitude                            f4.1
ms           Ms magnitude                            f4.1
mw           Mw magnitude                            f4.1
ntot         total number of defining observations   i4
ntel         number of defining teleseismic          i4
             observations (delta > 28 deg                   
ndep         number of defining depth phase          i4
             observations (delta > 28 deg)                   
igreg        Flinn-Engdahl region number             i4
se           standard error of observations used     f8.2
ser          standard error in position (km)         f8.2
sedep        standard error in depth (km)            f8.2
rstadel      distance to closest station             f6.1
openaz1      teleseismic azimuth gap                 f6.1
openaz2      secondary teleseismic azimuth gap       f6.1
az1          semi-axis azimuth                       f4.0
flen1        semi-axis length                        f4.1
az2          semi-axis azimuth                       f4.0
flen2        semi-axis length                        f4.1
             multiply product of len1 and len2 by pi 
             to get area of 90% confidence ellipse
avh          statistical geometric mean of axes      f5.1
ievt         ISC event identification number         i10

```
