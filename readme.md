Berkeley Lab WINDOW Calc Engine (CalcEngine) Copyright (c) 2016 - 2020, The Regents of the University of California, through Lawrence Berkeley National Laboratory (subject to receipt of any required approvals from the U.S. Dept. of Energy).  All rights reserved.

If you have questions about your rights to use or distribute this software, please contact Berkeley Lab's Innovation & Partnerships Office at IPO@lbl.gov.

NOTICE.  This Software was developed under funding from the U.S. Department of Energy and the U.S. Government consequently retains certain rights.  As such, the U.S. Government has been granted for itself and others acting on its behalf a paid-up, nonexclusive, irrevocable, worldwide license in the Software to reproduce, distribute copies to the public, prepare derivative works, and perform publicly and display publicly, and to permit other to do so.


# pywincalc

This module provides a simplified method for calculating various thermal and optical properties of glazing systems.

Version 2.0 has substantially more features but the interface has also changed as a result.  For help updating existing code see [Migrating to 2.0](migrating-to-2.0)


### Requirements
[Git](https://git-scm.com/)

[CMake](https://cmake.org/) - Not required for installing from wheel files on Windows.

## Install

### Linux
Once the requirements have been installed this can be installed with pip by doing

` pip install git+https://github.com/LBNL-ETA/pyWinCalc.git `

### Mac
Once the requirements have been installed this can be installed with pip by doing

` pip install git+https://github.com/LBNL-ETA/pyWinCalc.git `

### Windows
Wheels have been provided for 32 and 64 bit versions of Python 2.7 and 3.7.  To insall

```
git clone https://github.com/LBNL-ETA/pyWinCalc.git
cd pywincalc\wheels
pip install pywincalc-0.0.1-your-version-of-python.whl
```

For other versions of Python the correct C++ compiler first needs to be installed as well as CMake.  Once that has been installed pyWinCalc can be built following the Linux build steps.

## Use
Calculations can be performed with either a single solid layer or multiple solid layers separated by gaps.

A folder with example calculation script, products and standards is provided under the example directory.

### Units

With the exception of wavelength values which are in microns all units are values are in SI base units.  However for documentation some units are expressed as more common derived SI units when the values are equivalent.  For example:
- wavelengths: microns (m<sup>-6</sup>)
- conductivity: w⋅m<sup>-1</sup>⋅K<sup>-1</sup> because this is more common than m⋅kg⋅s<sup>−3</sup>⋅K<sup>−1</sup> and 1 w⋅m<sup>-1</sup>⋅K<sup>-1</sup> = 1 m⋅kg⋅s<sup>−3</sup>⋅K<sup>−1</sup>
- temperature: Kelvin
- pressure:  pascals
- thickness: meters
- width/height: meters
- etc...

### Optical Standards
Calculations can be performed using predefined optical standards in the form that is expected by [WINDOW](https://windows.lbl.gov/software/window).  The path to the base standard files is all that needs to be passed.  Any other files referenced by the standard file must be in the same directory (or specified as a relative directory from within the standard file).

Custom standards can be created by creating a new set of files following the same format.

### Solid layers
Solid layers define the glazing or shading products that make up a glazing system.  The methods for creating solid layers currently supported are:
 
- From paths to measured data files as exported by [Optics](https://windows.lbl.gov/software/optics).  See [single_clear.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/single_clear.py) and [triple_clear.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/triple_clear.py) in the examples directory.
- From json returned by a request for a shading layer to the [IGSDB](igsdb.lbl.gov).  See [igsdb_exterior_shade_on_clear_glass.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_exterior_shade_on_clear_glass.py) in the examples directory.
- From a combination of json returned by a request for a material from the [IGSDB](igsdb.lbl.gov) and user-defined geometry.  See [igsdb_custom_perforated.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_perforated.py), [igsdb_custom_venetian.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_venetian.py), and [igsdb_custom_woven.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_woven.py) in the examples directory.
- Shading layers that are represented by discrete BSDFs are currently a special case in the [IGSDB](igsdb.lbl.gov).  See [igsdb_interior_bsdf.py] (https://github.com/LBNL-ETA/pyWinCalc/blob/bsdf_input/example/igsdb_interior_bsdf.py)
- From measured wavelength data from some other source and user-defined geometry.  See [custom_perforated.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/custom_perforated.py) in the examples directory.

#### Supported solid layer types
The following types of solid layers are currently supported:
- Glazings that are represented as one set of measured wavelength data.  Products that require deconstruction like some laminates and coated glass are not yet supported.
- Venetian blinds
- Woven shades
- Perforated screens
- BSDF shades

### Gaps
For systems with more than one solid layer each solid layer must be separated by a gap.  The methods for creating gaps currently supported are:

- From a selection of predefined gases.  Current predefined gases are:  Air, Argon, Krypton, Xeon.
- From a mixture of predefined gases.
- From creating a custom gas by providing molecular weight, specific heat ratio, and coefficients for specific heat at constant pressure (Cp), thermal conductivity, and viscosity.
- From a mixture of custom gases.

For examples of each see [gases.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/gases.py) in the examples directory.

### BSDF Calculations

Shading products require BSDF calculations while glazings do not.  If any layer passed to a glazing system is a shading layer the glazing system will also require a BSDF hemisphere.  For examples see any of the igsdb examples or [custom_perforated.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/custom_perforated.py) in the examples directory.

However it is possible to use BSDF calculations for a system with no shading products.  To do so pass a BSDF hemisphere as in the examples with shading systems.

If a glazing system is given a BSDF hemisphere as a parameter it will always use that for optical calculations.

### Example use cases

Since there are several ways of creating and combining layers plus different calculation options example scripts are provided in the [/example](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/) directory.  

These scripts use optical standards provided in the [/example/standards](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/standards) directory.  Some scripts use measured data for example products provided in the [/example/products](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/products) directory.

A minimum example might look like this
```
import pywincalc

optical_standard_path = "standards/W5_NFRC_2003.std"
optical_standard = pywincalc.load_standard(optical_standard_path)

width = 1.0
height = 1.0

clear_3_path = "products/CLEAR_3.DAT"
clear_3 = pywincalc.parse_optics_file(clear_3_path)

solid_layers = [clear_3]
gaps = []

glazing_system = pywincalc.GlazingSystem(solid_layers, gaps, optical_standard, width, height)
print("Single Layer U-value: {u}".format(u=glazing_system.u()))
```

Please see the following examples which contain more information.

NOTE:  The igsdb examples require the python requests library and an API token for igsdb.lbl.gov.  An API token can be obtained by creating an account there.  See https://igsdb.lbl.gov/about/ for more information on creating an account.

- [single_clear.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/single_clear.py): Creates a single layer glazing system from a sample optics file.  Shows all thermal results, all optical results for a single optical method and some optical results from a second optical method.
- [triple_clear.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/triple_clear.py):  Creates a triple layer glazing system from sample optics files.  Creates two gaps, one with a single gas and one with a gas mixture.  Shows another example of optical results for each layer.
- [igsdb_exterior_shade_on_clear_glass.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_exterior_shade_on_clear_glass.py):  Creates two double-layer glazing systems with exterior shading products (Venetian blind and perforated screen).  Uses shading layers and glass from the [IGSDB](http://igsdb.lbl.gov).  
- [igsdb_custom_perforated.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_perforated.py) Creates a double layer glazing system with an exterior perforated screen.  The perforated screen uses a material from the [IGSDB](http://igsdb.lbl.gov) and a user-defined geometry describing the perforations.  The glass layer uses data from the [IGSDB](http://igsdb.lbl.gov)
- [igsdb_custom_venetian.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_venetian.py).  Creates a double layer glazing system with an exterior venetian blind.  The venetian blind uses a material from the [IGSDB](http://igsdb.lbl.gov) and a user-defined geometry describing the slats.  Also includes an example of how to change the distribution method used for calculating optical results for the shade.  The glass layer uses data from the [IGSDB](http://igsdb.lbl.gov)
- [igsdb_custom_woven.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/igsdb_custom_woven.py) Creates a double layer glazing system with an exterior woven shade.  The woven shade uses a material from the [IGSDB](http://igsdb.lbl.gov) and a user-defined geometry describing the thread layout.  The glass layer uses data from the [IGSDB](http://igsdb.lbl.gov)
- [custom_perforated.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/custom_perforated.py) Creates a double layer glazing system with an exterior perforated screen.  Shows an example of getting measured data from somewhere other than either the [IGSDB](http://igsdb.lbl.gov) or optics file.
- [gases.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/gases.py) Creates gases and gas mixtures from predefined gas types and custom gases created from gas properties.
- [minimum_example.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/minumum_example.py) The minimum example shown above.  Calculates the U-value for a single piece of generic clear glass.

If there is something you are trying to calculate that does not exist as an example yet please contact us.

### pywincalc objects

#### GlazingSystem
- Constructor:  
    - Requires parameters:  
        - optical_standard
        - solid_layers
    - Optional parameters:
        - gap_layers  Defaults to no gap layers.  If more than one solid layer is provided then len(solid_layers) - 1 gap_layers must be provided
        - width_meters  Defaults to 1.0 meters
        - height_meters  Defaults to 1.0 meters
        - environment  Defaults to NFRC U environment
        - bsdf_hemisphere  Defaults to no BSDF hemisphere. Required if any solid layers require BSDF calculations.
        - spectral_data_wavelength_range_method  Defaults to full wavelength range.
        - number_visible_bands  Defaults to 5.  Not used if spectral_data_wavelength_range_method is set to full. 
        - number_solar_bands  Defaults to 10.  Not used if spectral_data_wavelength_range_method is set to full. 
- Available calculation methods.
    - Thermal
        - u(theta=0, phi=0) Calculates the U-value for the system at incidence angle theta and phi
        - shgc(theta=0, phi=0) Calculates the SHGC value for the system at incidence angle theta and phi
        - layer_temperatures(TarcogSystemType, theta=0, phi=0)  Calculates the temperature at each layer based on the given TarcogSystemType (U or SHGC) at theta and phi incidence angle.  Returns a list of temperatures for each solid layer in K.  Note that the TarcogSystemType is specifying the calculation methodology for this calculation which is independed of the environment used to construct the GlazingSystem.  When U system is passed as a parameter the layer temperatures will be calculated for the given environments without taking solar radiation into account.  When SHGC system is passed as a parameter solar radiation is taken into account.
        - solid_layers_effective_conductivities(TarcogSystemType, theta=0, phi=0)  Calculates the effective conductivity for each solid layer based on the given TarcogSystemType (U or SHGC) at theta and phi incidence angle.  Returns a list of effective conductivities for each solid layer.  See note in layer_temperatures for the meaning of the TarcogSystemType parameter.
        - gap_layers_effective_conductivities(TarcogSystemType, theta=0, phi=0)  Calculates the effective conductivity for each gap layer based on the given TarcogSystemType (U or SHGC) at theta and phi incidence angle.  Returns a list of effective conductivities for each gap layer.  See note in layer_temperatures for the meaning of the TarcogSystemType parameter.
        - system_effective_conductivities(TarcogSystemType, theta=0, phi=0)  Calculates the effective conductivity for the entire system based on the given TarcogSystemType (U or SHGC) at theta and phi incidence angle.  Returns a single value.  See note in layer_temperatures for the meaning of the TarcogSystemType parameter.
    - Optical
        - optical_method_results(OpticalMethodType, theta=0, phi=0)  Calculates all optical results for the given OpticalMethodType at theta and phi incidence angle.  Returns an OpticalResults object containing all of the results.  See Optical Results section below.
        - color(theta=0, phi=0) Calculates color results and theta and phi incidence angle.  Returns a ColorResults object.  See the Color Results section in Optical Results below.

#### Optical Results
There are two types of optical results available: those calculated from any method that is not a color method and color results.  Color calculations are a special case and therefore have their own method used to calculate them and their own results structure.  The differences between color results and other optical results is discussed in the section below.

An OpticalResults object has two parts: system\_results and layer\_results.

system_results apply to the system as a whole and are objects nested as follows:  `system_results.side.transmission_type.flux_type` With the following options available for each

- side:  front, back
- transmission_type: transmittance, reflectance
- flux_type: direct_direct, direct_diffuse, direct_hemispherical, diffuse_diffuse
    - Note:  direct_hemisperical = direct_direct + direct_diffuse

E.g. the direct-diffuse front reflectance is `system_results.front.reflectance.direct_diffuse`

layer_results contain a list of results correspoding to each solid layer.  Results for both sides are provided for each layer.  Currently the only supported result per side is absorptance.  Absorptance is available for both direct and diffuse cases.

E.g the diffuse back absorptance for the first solid layer is `layer_results[0].back.absorptance.diffuse`

###### Color Results
The structure of color results is similar to, but different from, the structure of other optical results.  There are two main differences.  First individual layer results are not yet supported for colors.  And second instead of one value at each flux type (direct-direct, direct-diffuse, etc...) color results have RGB, Lab, and Trichromatic values.  Those represent the same result mapped into three common color spaces for convenience.  

So while for other results doing `results.front.transmittance.direct_direct` would return a single value for colors that returns an object that contains RGB, Lab, and Trichromatic objects.  E.g. to get the RGB blue value from a color result this is required: `results.front.transmittance.direct_direct.rgb.B`


#### Environments
An environment consists of two parts:  the inside and outside environment.  The exterior environment will be used as the environment before the first solid layer in the system and the interior environment will be used after the last solid layer in the system.  Each contains the same fields.  To use custom values for thermal calculations create an Environments object from inside and outside Environment objects.

Environment fields:
- air_temperature
- pressure
- convection_coefficient
- coefficient_model
- radiation_temperature
- emissivity
- air_speed
- air_direction
- direct_solar_radiation

Pre-constructed NFRC U and SHGC environments are available by calling `pywincalc.nfrc_u_environments()` and `pywincalc.nfrc_shgc_environments()`

#### Gases
There are several options for gases.  Gases can be created either from pre-defined gases (e.g. Air, Argon, etc...), by supplying physical parameters to create arbitrary custom gases, or by mixtures containing either predefined or custom gases.

##### Predefined gases
Gases can be created from a PredefinedGasType.  Current supported predefined gas types are: AIR, ARGON, KRYPTON, and XENON.

A gap layer can be created from a predefined gas type like so:
```
gap_1 = pywincalc.Gap(pywincalc.PredefinedGasType.AIR, .0127)  # .0127 is gap thickness in meters
```

#### Custom gases
A CustomGasData object can be created by providing the following information:
- Name
- molecular_weight
- specific_heat_ratio
- Cp
   - Expressed as a GasCoefficients object with A, B, and C fields.
- thermal_conductivity
    - Expressed as a GasCoefficients object with A, B, and C fields.
- viscosity
   - Expressed as a GasCoefficients object with A, B, and C fields.

See [gases.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/gases.py) for more on creating custom gases.

#### Gas mixtures
Gas mixtures can be created from custom and predefined gases by specifying the percentage of each in the mixtures.  E,g,

```
# The following creates a gas that is 80% sulfur hexafluoride, 15% Argonm and 5% Air
gap_4_component_1 = pywincalc.CustomGasMixtureComponent(sulfur_hexafluoride, 0.8)
gap_4_component_2 = pywincalc.PredefinedGasMixtureComponent(pywincalc.PredefinedGasType.ARGON, .15)
gap_4_component_3 = pywincalc.PredefinedGasMixtureComponent(pywincalc.PredefinedGasType.AIR, .05)
gap_4 = pywincalc.Gap([gap_4_component_1, gap_4_component_2, gap_4_component_3], .025)  # 2.5mm thick gap
```

See [gases.py](https://github.com/LBNL-ETA/pyWinCalc/blob/shading_calcs/example/gases.py) for more on creating gas mixtures.


### Migrating to 2.0

There were several interface changes that resulted from the new functionality.  These changes are mostly contained in two places:  The GlazingSystem constructor and the results structures.  Each section will start with a guide on how to convert existing code and will follow with some rational and explination.  This conversion will convert the code in the v1 example.py file to code that will work in v2.

```python 
# Code prior to line 16 in the v1 example.py does not need to be changed

# v1 code
glazing_system_single_layer = pywincalc.Glazing_System(solid_layers, gaps, standard, width, height)
u_results = glazing_system_single_layer.u() # calculate U-value according to ISO15099
print("Single Layer U-value: {u}".format(u=u_results.result))

# v2 code
glazing_system_single_layer = pywincalc.GlazingSystem(optical_standard=optical_standard, solid_layers=solid_layers, width=width, height=height, environment=pywincalc.nfrc_u_environments())
u_result = glazing_system_single_layer.u() # calculate U-value according to ISO15099
print("Single Layer U-value: {u}".format(u=u_result))

# These results are not available in the thermal results but are available in optical results
# See the section on available optical results for how to obtain them
print("Single Layer u t_sol: {t}".format(t=u_results.t_sol))
print("Single Layer u solar absorptances per layer: {a}".format(a=u_results.layer_solar_absorptances))

#v1 code 
shgc_results = glazing_system_single_layer.shgc() # calculate SHGC according to ISO15099
print("Single Layer SHGC: {shgc}".format(shgc=shgc_results.result))

# v2 code
# Important Note:  While it is still possible to calculate the SHGC value for the 
# glazing_system_single_layer system created above it will do so using the NFRC U 
# environments.  This will not result in the same SHGC as before.  To achieve the
# same SHGC as before a glazing system with the correct environment needs to be created

glazing_system_single_layer_nfrc_shgc_env = pywincalc.GlazingSystem(optical_standard=optical_standard, solid_layers=solid_layers, width=width, height=height, environment=pywincalc.nfrc_shgc_environments())
shgc_result = glazing_system_single_layer_nfrc_shgc_env.shgc() # calculate SHGC according to ISO15099
print("Single Layer SHGC: {shgc}".format(shgc=shgc_result))

# v1 code
# These results are not available in the thermal results but are available in optical results
# See the section on available optical results for how to obtain them
print("Single Layer SHGC t_sol: {t}".format(t=shgc_results.t_sol))
print("Single Layer SHGC solar absorptances per layer: {a}".format(a=shgc_results.layer_solar_absorptances))

#v1 code
optical_results_single_layer = glazing_system_single_layer.all_method_values(pywincalc.Method_Type.THERMAL_IR)

# v2 code
thermal_ir_optical_results_single_layer = glazing_system_single_layer.optical_method_results(pywincalc.OpticalMethodType.THERMAL_IR)

# v1 code
print("Single Layer Thermal IR optical transmittance front direct-direct: {r}".format(r=thermal_ir_optical_results_single_layer.direct_direct.tf))

# v2 code
print("Single Layer Thermal IR optical transmittance front direct-direct: {r}".format(r=thermal_ir_optical_results_single_layer.system_results..front.transmittance.direct_direct))

# v1 code
gap_1 = pywincalc.Gap_Data(pywincalc.Gas_Type.AIR, .0127) # .0127 is gap thickness in meters
gap_2 = pywincalc.Gap_Data(pywincalc.Gas_Type.ARGON, .02) # .02 is gap thickness in meters

# v2 code
gap_1 = pywincalc.Gap(pywincalc.PredefinedGasType.AIR, .0127)  # .0127 is gap thickness in meters
gap_2 = pywincalc.Gap_Data(pywincalc.PredefinedGasType.ARGON, .02) # .02 is gap thickness in meters

# v1 code
tf_rgb_results_triple_layer = color_results_triple_layer.direct_direct.tf.rgb
print("Triple Layer color results transmittance front direct-direct RGB: ({r}, {g}, {b})".format(r=tf_rgb_results_triple_layer.R, g=tf_rgb_results_triple_layer.G, b=tf_rgb_results_triple_layer.B))

# v2 code
tf_rgb_results_triple_layer = color_results_triple_layer.system_results.front.transmittance.direct_direct.rgb
print("Triple Layer color results transmittance front direct-direct RGB: ({r}, {g}, {b})".format(r=tf_rgb_results_triple_layer.R, g=tf_rgb_results_triple_layer.G, b=tf_rgb_results_triple_layer.B))
```

#### GlazingSystem


First Glazing_System was changed to GlazingSystem to be more in line with python's naming conventions.

Second the GlazingSystem constructor parameter order changed and there are additional paramters that can be passed in. All parameters are now able to be passed using keywords.  e.g.

`glazing_system = pywincalc.GlazingSystem(optical_standard=optical_standard, solid_layers=solid_layers)`

Finally there is a change to how environmental conditions are handled.  In v1 GlazingSystem had two built-in environments -- NFRC U and NFRC SHGC.  In v2 a GlazingSystem has one environment passed in as a parameter.  This allows for any environmental conditions to be used in thermal calculations.

For convenience there are methods to get the NFRC envionmnents, `pywincalc.nfrc_u_environments()` and `pywincalc.nfrc_shgc_environments()`  To create custom environments see the Environments section above.

However this means that some care should be taken when constructing glazing systems for thermal results.  The NFRC U and SHGC environments are simply standardized environmental conditions used by NFRC to generate their respective thermal result.  But any environmental conditions can be used to calculate both U and SHGC values.

For example, in the example.py file in v1 there is code to get U and SHGC values that looks like this 

```
u_results = glazing_system_single_layer.u() # calculate U-value according to ISO15099
shgc_results = glazing_system_single_layer.shgc() # calculate SHGC according to ISO15099
```

That behavior used to calculate U and SHGC from the respective built-in environments.  Now in order to do the equivilant two glazing systems need to be created:

```
glazing_system_nfrc_u_env = pywincalc.GlazingSystem(optical_standard=optical_standard, solid_layers=solid_layers, environment=pywincalc.nfrc_u_environments())
glazing_system_nfrc_shgc_env = pywincalc.GlazingSystem(optical_standard=optical_standard, solid_layers=solid_layers, environment=pywincalc.nfrc_shgc_environments())

nfrc_u_value = glazing_system_nfrc_u_env.u()
nfrc_shgc_value = glazing_system_nfrc_shgc_env.shgc()
```

#### Results

##### Optical Results
The `all_method_results` function has been renamed to `optical_method_results`.  It still requires an optical method but now also accepts optional theta and phi values for angular calculations.

The top level object returned by the `optical_method_results` now has two components: system\_results and layer\_results.  As the names imply system\_results contains results that apply to the system as a whole while layer\_results have results on a per-solid-layer basis.

Under system\_results then it goes side, transmittance or reflectance, then flux type (direct-direct, direct-diffuse, direct-hemispheric, or diffuse-diffuse)


##### Thermal Results

GlazingSystem.u() and GlazingSystem.shgc() now return single values
Both u() and shgc() take optional theta and phi values for angular calculations.  Both theta and phi default to zero so if neither are provided the result will be for normal incidence angle.



### Tutorial Videos

https://youtu.be/YQzCho-Vx-k

https://youtu.be/_lfoyZ2ntkU
