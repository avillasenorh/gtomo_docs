```
                            *HDF FILE FORMAT

*HDF file read statement

       read(1,100) 
     1 ahyp,isol,iseq,iyr,mon,iday,ihr,min,sec,ad,glat,glon,
     2 depth,iscdep,mb,ms,mw,ntot,ntel,ndep,igreg,se,ser,sedep,
     3 rstadel,ropenaz2,topenaz2,az1,flen1,az2,flen2,avh
 100  format(a1,a3,a2,i2,2i3,1x,2i3,f6.2,a1,2f8.3,2f6.1,3f4.1,
     1 4i4,3f8.2,3f6.1,4i4,f5.1)

Variable definitions:

ahyp         secondary az gap (teleseismic stations) a1

             blank <= 180 deg
             Z     >  180 deg

             or for pre-1964 events

             A     <= 180 deg
             B     <= 210 deg and  > 180 deg
             C     <= 240 deg and  > 210 deg
             D     <= 270 deg and  > 240 deg
             F     >  270 deg

isol         solution type                           a3

             HEQ = origin time & hypocenter fixed
             DEQ = depth free
             LEQ = depth fixed by program
             FEQ = depth fixed by Engdahl
             XEQ = poor solution

iseq         other info                              a2

             X  = explosion/cavity collapse
             Xx = location known to x km
             M  = focal mechanism available
             Mx = location known to x km
              D = depth reviewed and confirmed

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
iscdep       depth reported by ISC                   f6.1
mb           mb magnitude                            f4.1
ms           Ms magnitude                            f4.1
mw           Mw magnitude                            f4.1
ntot         total number of observations used       i4
ntel         number of teleseismic observations      i4
             (delta > 28 deg) used                   
ndep         number of depth phase observations      i4
             (delta > 28 deg) used                   
igreg        Flinn-Engdahl region number             i4
se           standard error of observations used     f8.2
ser          standard error in position (km)         f8.2
sedep        standard error in depth (km)            f8.2
rstadel      distance to closest station             f6.1
ropenaz2     secondary az gap (regional stations)    f6.1
topenaz2     secondary az gap (teleseismic stations) f6.1
az1          semi-axis azimuth                       f4.0
flen1        semi-axis length                        f4.1
az2          semi-axis azimuth                       f4.0
flen2        semi-axis length                        f4.1
             multiply product of len1 and len2 by pi 
             to get area of 90% confidence ellipse
avh          statistical geometric mean of axes      f5.1
```
