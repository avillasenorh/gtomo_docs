```
                            *HDF FILE FORMAT

*HDF file read statement

       read(1,100) 
     1 ahyp,isol,iseq,iyr,mon,iday,ihr,min,sec,ad,glat,glon,
     2 depth,iscdep,mb,ms,mw,ntot,ntel,ndep,igreg,se,ser,sedep,
     3 rstadel,openaz1,openaz2,az1,flen1,az2,flen2,avh
 100  format(a1,a3,a2,i2,2i3,1x,2i3,f6.2,a1,2f8.3,2f6.1,3f4.1,
     1 4i4,3f8.2,3f6.1,4i4,f5.1)

Variable definitions:

ahyp         secondary teleseismic azimuth gap       a1

             blank = <= 180 deg
             Z     = >  180 deg

                    or

             primary teleseismic azimuth gap

             A     = <  180 deg
             B     = <  210 deg and  > 180 deg
             C     = <  240 deg and  > 210 deg
             D     = <  270 deg and  > 240 deg
             F     = >  270 deg

isol         solution type                           a3

             HEQ = origin time & hypocenter fixed
             DEQ = depth free (sedep <= 15 km)
             WEQ = depth fixed at waveform depth
             BEQ = depth fixed at BB depth
             FEQ = depth fixed by Engdahl
             LEQ = depth fixed by program (sedep > 15 km)
             XEQ = poor solution (ser > 35 km)

iseq         other info (b=blank)                    a2

             bx = earthquake location known to x km (if x is a number)
             bc = only regional event data available
             Xb = explosion/cavity collapse
             Xx = explosion location known to x km (if x is a number)
             Mb = CMT solution available
             Mx = CMT location known to x km (if x is a number)
             Md = depth reviewed and accepted
             Mn = depth unreviewed but provisionally accepted 
                  based on => 5 depth phases and/or station(s) 
                  at distance(s) less than the focal depth
             Mr = depth under review
             Mf = depth set to regional depth estimate
             Mh = depth set to Harvard CMT depth
             Mx = depth reviewed but not accepted
             bd = depth reviewed and accepted
             bn = depth unreviewed, but provisionally accepted 
                  based on => 5 depth phases and/or station(s) 
                  at distance(s) less than the focal depth
             br = depth under review
             bf = depth set to regional depth estimate
             bx = depth reviewed but not accepted

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
ntot         total number of stations used           i4
ntel         number of teleseismic stations          i4
             (delta > 28 deg) used                   
ndep         number of depth phase observations      i4
             (delta > 28 deg) used                   
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
```
