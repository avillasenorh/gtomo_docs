```
                            *RES FILE FORMAT

*RES file read statement:

       read(1,100) 
     1 nev,isol,iseq,openaz2,ropenaz2,topenaz2
     1 iyr,imon,iday,ihold,ihr,imin,sec,
     1 elat,elon,depth,fmb,fms,ntot,ntel,
     2 sta,slat,slon,elev,delta,azim,
     3 comp,onset,phasej,iphj,iphi,ipho,
     5 rdtdd,rdelta,razim,dbot,
     6 gblat,gblon,stadel,bdep,tbath,twater,
     7 obstt,iprec,prett,rawres,
     8 ecor,scor,elcor,resid,iflg,wgt,
     9 tdelta,ttime,
     9 delisc,resisc,w
100   format(i7,1x,a3,a2,3f6.1,i5,2i3,i2,2i3,
     1 f6.2,2f8.3,f6.1,2f4.1,2i5,5x,
     1 1x,a6,2f8.3,f7.3,2f8.3,5x,
     2 1x,a2,1x,a1,1x,a8,3i4,5x,
     3 f8.4,2f8.3,f7.1,5x,
     3 3f8.3,f7.3,2f7.2,5x,
     4 f10.2,i3,f10.2,f7.2,5x,
     5 4f7.2,i2,f5.2,5x,
     6 f8.3,f10.2,5x,f8.3,f7.2,1x,a1)

Variable definitions:

nev          sequential event number                 i7
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
             Xb = explosion/cavity collapse
             Xx = explosion location known to x km (if x is a number)
             Mb = CMT solution available
             Mx = CMT location known to x km (if x is a number)
             bd = depth reviewed and accepted
             br = depth under review
             bx = depth reviewed but not accepted
openaz2      secondary az gap (all stations)         f6.1 
ropenaz2     secondary az gap (regional stations)    f6.1
topenaz2     secondary az gap (teleseismic stations) f6.1
iyr          year                                    i5
imon         month                                   i3
iday         day                                     i3
ihold        0 = free depth solution                 i2
             1 = fixed depth solution                
             2 = fixed hypocenter solution            
ihr          origin hour                             i3
imin         origin minute                           i3
sec          origin second                           f6.2
elat         origin GEOCENTRIC latitude              f8.3
elon         origin GEOCENTRIC longitude             f8.3
depth        origin depth                            f6.1
fmb          origin mb magnitude                     f4.1
fms          origin Ms magnitude                     f4.1

ntot         number of stations used                 i5
ntel         number of teleseismic stations used     i5
             (tdelta >= 28 deg)                  

--------------------------------------------------------

sta          station code                            a6
slat         station GEOCENTRIC latitude             f8.3
slon         station GEOCENTRIC longitude            f8.3
elev         station elevation (km)                  f7.3
delta        distance to station                     f8.3
azim         azimuth to station                      f8.3

---------------------------------------------------------

comp         instrument & component                  a2
onset        phase onset(i,e, )                      a1
phasej       assigned phase name                     a8
iphj         assigned numerical phase id             i4
             (999 if unknown)
iphi         ISC numerical phase id                  i4
             (999 if unknown)
ipho         reported numerical phase id             i4
             (999 if unknown)

---------------------------------------------------------

rdtdd        phase ray parameter                     f8.4
rdelta       phase distance to station (major arc)   f8.3
razim        phase ray azimuth (azimuth to station)  f8.3
dbot         ray bottoming depth                     f7.1                     
             (for selected phases only)              

---------------------------------------------------------

gblat        bounce point GEOCENTRIC latitude        f8.3
gblon        bounce point GEOCENTRIC longitude       f8.3
stadel       distance to station from bounce point   f8.3
bdep         bounce point topo(-) or bath(+) in km   f7.3
tbath        bounce point crustal correction         f7.2
             (subtracted from observed travel time)         
twater       bounce point water travel time          f7.2
             (subtracted from observed travel time)  

---------------------------------------------------------
obstt        observed travel time (uncorrected)      f10.2
iprec        arrival time reading precision          i3
             3  to nearest 1/10th minute
             2  to nearest minute
             1  to nearest ten seconds
             0  to nearest second
            -1  to 1/10th second
            -2  to 1/100th second
prett        predicted travel time (uncorrected)     f10.2
rawres       observed residual (uncorrected)         f7.2

---------------------------------------------------------

ecor         station elevation correction            f7.2
             (subtracted from observed travel time)
scor         patch 'station' correction              f7.2
             (subtracted from observed travel time) 
             FOR GLOBAL TOMOGRAPHY scor SHOULD BE ADDED TO 
             ALL RELEVANT PARAMETERS BEFORE INVERSION OR ELSE 
             THE UPPER MANTLE WILL BE WHITENED!
elcor        ellipticity correction                  f7.2
             (subtracted from observed travel time)
resid        observed residual                       f7.2
             (999.99 if unknown)
iflg         0 = first arriving P phase used         i2
             1 = first arriving P phase not used
             2 = later phase used
             3 = later phase not used                
wgt          residual weight                         f5.2
             ( = 0 if not used)

---------------------------------------------------------

tdelta       surface focus distance                  f8.3
             ( = 0 for upgoing phases)
ttime        observed surface focus travel time      f10.2
             (corrected) ( = 0 for upgoing phases)

---------------------------------------------------------

delisc       ISC distance to station                 f8.3
             (999.999 if unknown)
resisc       ISC observed residual                   f7.2
             (999.99 if unknown)

---------------------------------------------------------

w            Arrival time read from waveform         a1

---------------------------------------------------------
```
