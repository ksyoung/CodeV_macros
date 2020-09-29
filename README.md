# CodeV_macros
Macros useful for designing and analyzing CMB optical systems.

See the CodeV manual for syntax and details on how to run macros. A brief outline on how 
to use these macros follows.

##### x_dragone_design.seq  
This macro takes 5 fundamental dragone parameters; aperture diameter, 
θe, θ0, θp, and primary to secondary distance. See Chang and
Prata 1999 for parameter definitions. The values can be entered when the macro is run
or the code can be edited each time. The code then calculates all values needed to
model the system in CodeV, prints them to the Command Window, and (if ^do is 1, you can 
change it manually in code) writes the values into the lens data manager. For this third 
step to work a lens with 7 surfaces must be defined. Surfaces 3 and 6 are mirrors. The 
rest are dummy surfaces.


##### greg_design.seq  
This macro takes 5 fundamental dragone parameters; aperture diameter, 
main reflector focal length, main reflector vertical offset, sub-reflector diameter,
and angle β. See Granet 2002 for parameter definitions. The values are entered when the 
macro is run. The code then calculates all values needed to model the system in CodeV, 
prints them to the Command Window, and writes the values into the lens data manager. For 
this third step to work a lens with 7 surfaces must be defined. Surfaces 3 and 6 are 
mirrors. The rest are dummy surfaces.

##### Strehl_grid.seq  
This macro calculates the Strehl ratio across a grid of points covering
a square section of the FOV which is centered on the chief ray. Inputs are: (1) The
number of points in the grid, which must be a square number; (2) The output filename;
and (3) The field angle at one edge of the grid. It works best to run this from the macro
manager GUI as it causes the macro to prompt for these inputs. Strehl is calculated
using CodeV’s ‘quick best focus’ tool which is fast, but can be inaccurate or fail at low
Strehl ratios, less than 0.5. To use the PSF tool to calculate Strehl instead comment
out the ‘quick best focus’ sections of the code and uncomment the PSF sections.

The Strehl data is printed to the Command Window and saved to the output file as
a csv. The output file has columns X angle (deg), Y angle (deg), X image height (cm),
Y image height (cm), Strehl ratio, and ‘flag’. The flag is 1 if the calculation succeeded
and 0 if it failed. Typically it fails near the corners of the grid where the radial distance
from FOV center is large and aberations or vignetting make ray tracing difficult.

##### Plot_Strehl_3freq.seq 
This macro calculates the Strehl ratio across the X and Y
axes of the focal surface for three different frequencies, plots the results, and solves
for where Strehl=0.8 along each axis. This gives four points which define an ellipse
that gives a good approximation of the total DLFOV. Just as with Streh_grid.seq
this macro uses ‘quick best focus’ but can be switched to use PSF. It runs much faster
than Streh_grid.seq with either setting. Inputs are: (1) The maximum field angle to
calcualate to in each direction, ±X and ±Y; (2-4) The calculation frequencies; (5) A
vestigial argument currently unused; (6) The subtitle for all plots; and (7) The Strehl
value which defines the DLFOV. This macro is easiest to use when run from the macro
manager GUI.

Additionaly, for this macro to run all 25 field angles must be ‘active’, i.e. have
a value entered in the Lens Data Manager, and the CodeV function @linint must be loaded. 
To load @linint run the CodeV provided macro flnint.seq from the Command Window by typing 
run "C:\CODEV106_SR1\macro\flnint.seq". And a final caveat, the interpolation which finds 
where Strehl=0.8 will fail if no Strehl value less than 0.8 is found. However, all the 
plots of Strehl vs field angle will still be produced and all Strehl values are printed 
to the Command Window

##### IP_PR_mueller.seq
This macro uses the CodeV pupil averaged Mueller Matrix macro
to calculate the polarization properties, instrumental polarization (IP) and polarization
rotation (PR), at the image surface for a square grid of input field angles. The macro 
saves the derived polarization properties and the Mueller Matrix components to csv output 
files. Inputs are: (1) The number of points in the grid, which must be a square number; 
(2) The polarization properties output filename; (3) The Mueller components output 
filename; and (4) The calculation frequency in GHz. The size of the calculation grid in 
angle is set by the Y field angle (YAN) of field 2 (F2). Again, I recommend the macro 
manager GUI. It will prompt for all inputs except the size of the grid in angle.

The first output file has columns X angle, Y angle, IP, PR, and ‘flag’ and the second
has columns X angle, Y angle, II, IQ, IU, QQ, QU, and ‘flag’. All angles are in degrees.
The flags are 1 if the calculation succeeded, and 0 if it failed.

##### fnum_plate_scale.seq  
This macro uses the CodeV PSF command to calculate the
system’s f-number and plate scale across a square grid at the focal surface. The f-number
is the average of the X and Y f-numbers at each point and the plate scale is derived
from that f-number. The inputs are: (1) The number of points in the grid, which must
be a square number; (2) The output filename; and (3) The field angle at one edge of
the grid. Run this from the macro manager GUI also.

The output file has columns X angle (deg), Y angle (deg), f-number, plate scale
(arsec/cm), and ‘flag’. The flag is 0 when the calculation fails and 1 when it succeeds.
