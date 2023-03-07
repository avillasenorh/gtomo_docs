# Ray tracing

The first steps of preprocessing consist of reading the EHB `RES` files,
verifying that some of the fields are correct, and writing out only the
relevant fields for the tomography. These steps are described using the
term **ray tracing** because one of the checks is that the ray-tracing
parameters in the `RES` files (distance, azimuth, travel time, ray parameter),
calculated with Buland and Chapman's codes are close enough to the values
obtained with the 1-D ray tracing routines used in the tomography codes.

The main checks carried out by these pre-processing codes are:

- Verify that the distance and azimuth listed in the `RES` files are
  similar to the values calculated but the tomography code routines.
  In particular verify that the distance is calculated correctly for
  phases that travel for more that 180 degrees (e.g. P'P'?).
- For the phases that have a bounce in the free surface or ocean bottom,
  verify that the coordinates of the bounce point listed in the `RES` files
  coincides with the one calculated with the tomography codes.
- Verify that the ellipticity correction listed in the `RES` files is similar
  to the ellipticity correction calculated by the subroutine `ellip`.
- Verify that the ray parameter and travel times listed in the `RES` files
  are similar to the ones calculated by the 1-D ray tracing routines used
  in the tomography codes.
- Write out the uncorrected travel time residual (without the patch correction)
  so it contains the full contribution from the 3D structure.

The original sequence of programs from `RES` files to *checked* files was the following:

```
input        code            output

RES      ->  preprocess  ->  PREdata
PREdata  ->  read        ->  RDdata
RDdata   ->  ellipt      ->  *ELP
*ELP     ->  trace_bob   ->  *TRACE
*TRACE   ->  checkdata   ->  *CHK
```

To reduce the number of input and output files, new pre-processing codes were written:

- `res2trace2`: combines `preprocess`, `read`, and `ellipt`
- `??`: combinets `trace*`, and `checkdata`

The final output format of all this processing is:

```fortran
	write(6,111) 
     >            ievent,idate,ihour,iminut,sec,
     >            flon,flat,hf,iihold,xmb,xms,
     >            bop,top,xln1,yln1,xln2,yln2,
     >            ntele,ista,xst,yst,delta,derr,disc,az,
     >            ionset,idm1,idx,iph2,iph3,
     >            sfd,sft,tr,terr,ryp,pr,delay1,delay,risc,ifl,
     >            rbd,rb,dbs,dbstat,ucor,ecor,topocor,evcor,prec,
     >            ierror,ierr2,iwhat
111     format(i7,1x,i6,1x,2(i2,1x),1x,f5.2,1x,2(f8.2,1x),f4.0,
     >         1x,i1,1x,2(f3.1,1x),f4.1,1x,f5.1,1x,4(f8.2,1x),
     >         1x,i3,1x,i6,1x,2(f8.2,1x),1x,
     >         3(f6.2,1x),1x,f6.2,1x,i1,1x,4(i3,1x),1x,f6.2,1x,
     >         f7.2,1x,4(f9.2,1x),1x,2(f6.2,1x),f6.1,1x,i1,1x,
     >         f6.0,1x,
     >         f9.2,1x,6(f6.2,1x),i3,1x,i2,1x,i1,1x,i1)
```

The meaning of the variables is:

- `ievent`: event number (un-modified from the original `RES` files)
- `idate`: event origin time date in YYMMDD format (non-Y2K!)
- `ihour`, `iminut`, `sec`: event origin time hour, minutes, and seconds
- `flon`, `flat`: geocentric longitude and latitude of the event (degrees or radians?)
- `hf`: event focal depth in km
- `iihold`: solution type (0=free, 1=fixed depth, 2=fixed hypocenter)



One last step is program `select`


Input

```fortran
        read(5,11,err=1111,end=5)
     >            ievent,idate,ihour,iminut,sec,
     >            flon,flat,hf,iihold,xmb,xms,
     >            bop,top,xln1,yln1,xln2,yln2,
     >            ntele,ista,xst,yst,delta,derr,disc,az,
     >            ionset,idm1,idx,iph2,iph3,
     >            sfd,sft,tr,terr,ryp,pr,delay1,delay,risc,ifl,
     >            rbd,rb,dbs,dbstat,ucor,ecor,topocor,acc,
     >            ierror,iwhat
11      format(i7,1x,i6,1x,2(i2,1x),1x,f5.2,1x,2(f8.2,1x),f4.0,
     >         1x,i1,1x,2(f3.1,1x),f4.1,1x,f5.1,1x,4(f8.2,1x),
     >         1x,i3,1x,i6,1x,2(f8.2,1x),1x,
     >         3(f6.2,1x),1x,f6.2,1x,i1,1x,4(i3,1x),1x,f6.2,1x,
     >         f7.2,1x,4(f9.2,1x),1x,2(f6.2,1x),f6.1,1x,i1,1x,
     >         f6.0,1x,
     >         f9.2,1x,5(f6.2,1x),f3.1,1x,i2,1x,i1)
```

Variables not written:

xms
bop
top
xln1, yln1, xln2, yln2
xst, yst
ntele


Output:

```fortran
c       output format of Forward/1_*/Src/trace_data.f
        write(6,111) 
     >          ievent,idate,ihour,iminut,sec,
     >          flon, flat, hf,iihold,xmb,
c    >          ista,xst,yst,delta,az,
     >          ista,        delta,az,
     >          ionset,idm1,idx,
     >          tr,pr,delay,ifl,
     >          ierror,iwhat
111    format(i7,1x,i6,1x,2(i2,1x),1x,f5.2,1x,2(f8.2,1x),f4.0,
c    >          1x,i1,1x,f3.1,1x,i6,(2f8.2,1x),f7.2,1x,f6.1,1x,i3,
     >          1x,i1,1x,f3.1,1x,i6,           f7.2,1x,f6.1,1x,i3,
     >          i4,1x,i3,1x,3(f8.2,1x),1x,(3I3))

```
