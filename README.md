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

Klipper support the LCD and LED.

**Installing Klipper on a Creator Pro:**

https://user-images.githubusercontent.com/49697720/154837388-ef2b174c-0af7-4918-bf0e-0fd06683107a.mp4

The provided macros have been implimented in a slicer neutral way, tested against the latest releases of:

- Cura         
- PrusaSlicer  
- SuperSlicer

GCode start and end macros are provided (START_PRINT & END_PRINT). The start macro utilises slicer variables to determine how many extruders are active so you don't have to change the start code when switching between dual and single (either extruder) extrusion, PROVIDED you configure the slicer as defined below.

If you are utilising a probe, the start print macro supports bed leveling by setting the MESH parameter top true i.e. MESH=true. If requested it will do the following:

- Load a saved mesh for the defined bed temperature, if it exists else
- Load the saved default mesh, if it exists else
- Generate a new mesh

Extruder offsets are defined in the ACTIVATE_EXTRUDER overriden macro, DO NOT define them in your slicer without changing this macro.

- ## Cura

Start GCode:
```
START_PRINT BED={material_bed_temperature_layer_0} EXTRUDER={material_print_temperature_layer_0, 0} EXTRUDER1={material_print_temperature_layer_0, 1} MESH=false EXTRUDERS_ENABLED={extruders_enabled_count} INITIAL_EXTRUDER={adhesion_extruder_nr}
```

End GCode:
```
END_PRINT
```
## PrusaSlicer

## SuperSlicer



