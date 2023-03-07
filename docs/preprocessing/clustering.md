# Clustering


Input format for `event_cluster`:


```fortran
         read(11,*,end=2,iostat=ios)
     >            ievent,idate,ihour,iminut,sec,
     >            flon,flat,hf,iihold,xmb,
     >            ista,delta,az,
     >            ionset,iphase,idx,
     >            tr,pr,delay,ifl,
     >            ierror,iwhat
```

- ievent                           event number
- idate,ihour,iminut,sec           date (Y2K?), hour, minute, second
- flon,flat,hf                     latitude (deg?, geocentric?), longitude (deg?, geocentric?), depth (km)
- iihold                           0, 1, 2 (free, fixed depth, fixed hypocenter)
- xmb                              mb magnitude
- ista                             station index (according to num_all_sta?)
- delta,az                         distance and azimuth (deg?)
- ionset                           0=I, 1=E, 2=blank
- iphase,idx                       phase_id (iphj), branch 
- tr,pr,delay                      travel time, ray parameter, residual (without corrections)
- ifl
- ierror,iwhat     ierror always 0?, iout=iwhat on output to distinguish refractions

