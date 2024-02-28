# Foreword
This project comes as a result of my desire to create a companion tool for Nathan Okun's wonderful armor penetration calculator, FACEHARD, which can be found at this link:
http://www.navweaps.com/index_nathan/index_nathan.php

FACEHARD is perhaps the highest fidelity tool in existence for predicting the penetration of gun-fired projectiles in 19th and 20th century naval armors. However, it requires knowing the angle and velocity with which a shell strikes its target in order to be used, which is not a problem that can be solved analytically due to the influence of forces such as drag. Historically, the solution to this issue was experimental verification, but live-fire testing of large-calibre naval rifles in the modern day is obviously quite impractical. Therefore, this calculator was devised to provide a highly accurate numerical solution to this problem.

Finally I cannot in good faith fail to mention that Mr. Okun has sadly passed away in January of 2024. While I never personally interacted with him, I harbor a deep professional respect for his contributions to the naval history community, and many naval historians who did know him personally, such as Jon Parshall, seem to hold Mr. Okun in high regard.

# What Does it Do?
This calculator provides a forward numeric integration of a projectile fired from a gun, currently using the 4th order RKF method with an embedded 5th order integration for dynamic stepsize control. Boat tail projectiles are not modelled, but base bleed options are included. The projectile must also be radially symmetric, and is assumed to fly at zero angle-of-attack. Elevation or other terrain is not modelled. The gun is assumed to be fired on an object in hydrostatic equilibrium and with a spheroid shape. Projectiles fired on escape trajectories are not modelled. Coriolis, centrifugal, and Euler forces are modelled.

The calculator also contains a software implementation of the International Standard Atmosphere (ISA), as well as the Buck and Antoine equations for the saturation vapour pressure. Drag is also accurately modelled for the class of projectile assumed.

# RKF4(5) User Guide
This calculator uses the 4th order Runge-Kutta method, with an embedded 5th order method for stepsize and erroor control. To run it, download the file and copy it into your MATLAB installation. Sadly, I cannot promise the file will work in Octave.

## Parameters
This calculator is highly flexible, and is suitable for use with a variety of projectile assumptions. Additionally, the editability of the planet and atmosphere parameters makes the calculation suitable for use on exoplanets.

### Planetary Parameters
Located in the first block of code. Default values for Earth. Inputs:
- Mass of the planet for which the simulation is run
- Semi-major axis of the planet
- Inverse flattening of the planet
- Sidereal day length of the planet in seconds
- Options for manually editing the planet angular velocity and angular acceleration vectors (used for Coriolis, centrifugal, and Euler force calculations)

Sources:
- Jekeli, Christopher (August, 2016). Geometric Reference Systems in Geodesy. Ohio State University.
- TOPEX/Poseidon (1992)

### Atmosphere Parameters
Second block of code. Default values for ISA. Inputs:
- Atmospheric pressure of dry air at reference altitude
- Molar mass of dry air
- Adiabatic constant of dry air
- Relative humidity for which the simulation is run (water vapour partial pressure is automatically calculated)
- Options for humidity calculation using either Buck or Antoine formulae
- Reference temperature at reference altitude
- Reference altitude
- Layers modelling temperature lapse and inversions with altitude

Sources:
- Bridgeman, O. C. & Aldrich, E. W. (1964). Vapor Pressure Tables for Water, J. Heat Transfer, 1964, 86, 2, 279-286, https://doi.org/10.1115/1.3687121
- Gubkov, A. N.; Fermor, N. A.; et al. (1964). Vapor Pressure of Mono-Poly Systems, Zh. Prikl. Khim. (Leningrad), 1964, 37, 2204-2210
- ISA standard atmosphere
- Liu, C. T. & Lindsay, W. T., Jr. (1970). Vapor Pressure of D2O from 106 to 300 ºC, J. Chem. Eng. Data, 1970, 15, 4, 510-513, https://doi.org/10.1021/je60047a015
- NIST Webbook https://webbook.nist.gov/cgi/cbook.cgi?ID=C7732185&Mask=4&Type=ANTOINE&Plot=on
- Stull, Daniel R. (1947). Vapor Pressure of Pure Substances. Organic and Inorganic Compounds, Ind. Eng. Chem., 1947, 39, 4, 517-540, https://doi.org/10.1021/ie50448a022

### Projectile Parameters
Third block of code. Default values for the American Mk. VII 16"/50 naval rifle. Inputs:
- Shell mass, diameter, and length
- Length of the shell's nose
- Diameter of the rounded tip of the shell's nose (also can be used to account for hemispherical noses)
- Nozzle diameter and burn time (allows for base bleed projectiles to be modelled)
- Muzzle velocity
- Firing solution (azimuth and elevation)

Sources: 
- Fleeman, Eugene L. (4th August, 2006). Tactical Missile Design, Second Edition. AIAA Education Series.
- NavWeaps http://www.navweaps.com/

### Ship Parameters
Fourth block of code. Also converts the firing ship location and projectile velocity vector into the initial state vector used for the simulation. Inputs:
- Latitude of the firing ship
- Longitude of the firing ship

Sources:
- Jekeli, Christopher (August, 2016). Geometric Reference Systems in Geodesy. Ohio State University.

### Other Sources
The iterative root-finding algorithm used to convert the projectile's ECEF position into geodetic coordinates are taken from Fukushima, Toshio (December, 1999). Fast transform from geocentric to geodetic coordinates. Journal of Geodesy. National Astronomical Observatory.

## Output
The following values are provided:
- Striking velocity, in feet per second
- Striking angle, both against a horizontal deck and a completely vertical (unangled) belt
- Latitude and longitude of the impact point

Additionally, a graph depicting the projectile trajectory, as well as the user-defined spheroid, is provided.


# Status
Currently under development.

A partly-functional RKF4(5) solver is complete, but only provides the striking velocity, striking angle, and trajectory of the user-determined fire control solution.

A fully-functional RKF7(8) solver is under development. The user will be able to use this solver in a far more practical manner by instead inputting the location and course of a target vessel, and having the calculator determine the fire control solution iteratively. The user will then be shown the trajectory associated with the final fire control solution, as well as associated hit probabilities.

Options for wind compensation may come in the future as well.
