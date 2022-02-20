# Flashforge Creator Pro

## Introduction

Creator Pro upgrade project, primarily focused on using Klipper with the stock board, a version of the MightyBoard. Hopefully this project is of some use to others.

I have several printers, a mix of mainstream hobbyist machines and self-built. I use my Creator to print higher temperature materials, predominantly ABS/ASA, single and dual extrusion.

I've undertaken several modifications, to improve its performance, some can be done using the stock firmware:

- TMC22nn (2208 in my case) drivers
- All metal hotends
- Different build plate

In addition, through upgrading the firmware on the stock board to Klipper, I've added:

- Probe (inductive)
- Filament sensors  

## Klipper

I've produced a video that walks you through:

- Building a Mainsail installation
- Generating and flashing the Klipper firmware
- Configuring Klipper using my ![configuration](klipper/config/) files 

The printer configuration file has my printer settings for input shapping, PID calibration and pressure advance.

Klipper support the LCD and LED.

**Installing Klipper on a Creator Pro:**

https://user-images.githubusercontent.com/49697720/154837388-ef2b174c-0af7-4918-bf0e-0fd06683107a.mp4

The provided macros have been implimented in a slicer neutral way, tested against the latest releases of:

- Cura         
- PrusaSlicer  
- SuperSlicer

GCode start and end macros are provided (START_PRINT & END_PRINT). The start macro utilises slicer variables to determine how many extruders are active so you don't have to change the start code when switching between dual and single (either extruder) extrusion, PROVIDED you configure the slicer as defined below.

If you are utilising a probe, the start print macro supports bed leveling by setting the MESH parameter to true i.e. MESH=true. If requested it will do the following:

- Load a saved mesh for the defined bed temperature, if it exists else
- Load the saved default mesh, if it exists else
- Generate a new mesh

Extruder offsets are defined in the ACTIVATE_EXTRUDER overriden macro, DO NOT define them in your slicer without changing this macro.

Copy and paste code to your slicer:

- ## Cura

Start GCode:
```
START_PRINT BED={material_bed_temperature_layer_0} EXTRUDER={material_print_temperature_layer_0, 0} EXTRUDER1={material_print_temperature_layer_0, 1} MESH=false EXTRUDERS_ENABLED={extruders_enabled_count} INITIAL_EXTRUDER={adhesion_extruder_nr}
```
To avoid changing start gcode when changing between single or dual extruders, disable / enable extruders that will be active / inactive via the Cura interface i.e.


![cura](https://user-images.githubusercontent.com/49697720/154839521-d1f144b9-40a8-490c-9b33-ff83ac3d6004.png)

End GCode:
```
END_PRINT
```

## PrusaSlicer

Start GCode:
```
# M140 S0   ; 
# M104 S0 T0; Stop slicer generating its own heater settings
# M104 S0 T1;
# START_PRINT BED=[first_layer_bed_temperature] EXTRUDER={first_layer_temperature[0]} EXTRUDER1={first_layer_temperature[1]} MESH=false EXTRUDERS_ENABLED={(filament_preset[0] != filament_preset[1]? 2 : 1)} INITIAL_EXTRUDER=[initial_extruder]
```

End GCode:
```
END_PRINT
```
To avoid changing start gcode when changing between single or dual extruders, when only utilising one extruder set the filament to be the same for both extruders.

## SuperSlicer (Prusa fork)

Note: SuperSlicer supports generating native Klipper commands if you set firmware to klipper

Start GCode:
```
# M140 S0   ; 
# M104 S0 T0; Stop slicer generating its own heater settings
# M104 S0 T1; Not required if firmware set to klipper
# START_PRINT BED=[first_layer_bed_temperature] EXTRUDER={first_layer_temperature[0]+extruder_temperature_offset[0]} EXTRUDER1={first_layer_temperature[1]+extruder_temperature_offset[1]} MESH=false EXTRUDERS_ENABLED={(filament_preset[0] != filament_preset[1]? 2 : 1)} INITIAL_EXTRUDER=[initial_extruder] 
```

End GCode:
```
END_PRINT
```
To avoid changing start gcode when changing between single or dual extruders, when only utilising one extruder set the filament to be the same for both extruders.
