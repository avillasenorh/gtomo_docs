# EHB bulletin

The most suitable database of earthquake locations and seismic phases for
global travel time tomography is the EHB bulletin. The main characteristics
of this bulletin are the following:

- uses a modern Earth model (ak135)
- uses depth phases pP, pwP and sP for depth determination (and these phases are
  dynamically re-identified)
- takes into account the Earth's 3D structure through the use of patch corrections

## Versions of the bulletin

The original EHB bulletin (Engdahl et al. 1998) included nearly 100,000 events
from 1964 to 1995 that were well constrained with teleseismic phases.

Subsequently Bob Engdahl produced updated versions of the EHB bulletin, including
also events well constrained with regional phases. These bulletins were named
EHB02 (1964-2000), EHB04 (1964-2004), and EHB08 (1960-2008).

More recently the [ISC](http://www.isc.ac.uk) has taken over the task of continuosly updating the EHB bulletin
(Weston et al., 2018; Engdahl et al., 2020) and the data is publicly available at the
[ISC-EHB web page](http://www.isc.ac.uk/isc-ehb/).


## Data formats

### `RES` file


The format of the `RES` files in the ISC-EHB bulletin is described [here](isc-ehb_res_format.md).

- EHB RES format
- EHB02 [RES format](ehb02_res_format.md)
- EHB04 [RES format](ehb04_res_format.md)
- EHB08 [RES format](ehb08_res_format.md)
- ISC-EHB [RES format](isc-ehb_res_format.md)

Coordinates in the `RES` files are in geocentric colatitude \( \theta^\prime \in [0^\circ,180^\circ] \)
and geocentric longitude \( \lambda^\prime \in [0^\circ,360^\circ) \). Not that in the descriptions
of the `RES` format it is **incorrectly** stated that coordinates are in geocentric latitude (they
are in geocentric **colatitude**).

## `HDF` file

- EHB HDF format
- EHB04 [SEL format](ehb04_sel_format.md)
- EHB08 [HDF format](ehb08_hdf_format.md)
- ISC-EHB [HDF format](isc-ehb_hdf_format.md)

Coordinates in the `HDF` files are in geographic (geodetic) latitude \( \varphi \in [-90^\circ, 90^\circ] \)
and geographic longitude \( \lambda \in (-180^\circ,180^\circ] \).

### Station file

The latest EHB datasets (EHB08 and ISC-EHB) also include a station file
`stations_ehb`, with station codes and coordinates.

## References
Engdahl, E. R., van der Hilst, R. D., & Buland, R. (1998). Global Teleseismic
Earthquake Relocation with Improved Travel Times and Procedures for Depth
Determination. Bull. Seismol. Soc. Am., 88(3), 722–743.

Weston, J., Engdahl, E. R., Harris, J., Di Giacomo, D., & Storchak, D. A. (2018).
ISC-EHB: reconstruction of a robust earthquake data set. Geophysical Journal
International, 214(1), 474–484. http://doi.org/10.1093/gji/ggy155.

Engdahl, E. R., Di Giacomo, D., Sakarya, B., Gkarlaouni, C. G., Harris, J., &
Storchak, D. A. (2020). ISC‐EHB 1964–2016, an Improved Data Set for Studies of
Earth Structure and Global Seismicity. Earth and Space Science, 7(1),
e2019EA000897. http://doi.org/10.1029/2019EA000897.
