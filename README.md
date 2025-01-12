# crayonest
Tools for assessing and comparing crayons

The following describes the process for creating, assessing, and comparing a crayon.

1. Copy the files in an existing crayon folder to a new folder (which I'll call C:\<this_crayon>). Do not change the names of files. 
  a. shapes.geojson
  b. stops.geojson
  c. frequencies.csv
    - Note that frequency is in trains/buses per hour
  d. meta.csv
  e. service_windows.csv
2. Edit the shapes and stops files in QGIS or some other editor to define the lines and stops and edit the lines in frequencies.csv.
  - make_gtfs probably won't accept your geojson file if you create it from scratch. Copy the existing files, then edit them (or try your luck).
  - Create as many new lines as you like.
  - Give each line a unique shape_id (Change the shape_id parameter for each line in the shapes.geojson file to the name of the line).
  - Only stops to the right of the line are registered as stops.
    o Stops have to be within a few metres of the line on the right side (the side matters only where lines travel in one direction, normally stops go on both sides).
    o Be careful at transfer stations or where lines cross not to create extra stops.
    o Stops have to be on both sides of the line for the stop to be registered as a stop in both directions.
  - Add a line in frequencies.csv for each shape_id in shapes.geojson.
  - Optionally, also edit speed_zones.geojson to define speed limits (useful for when a line has different speeds on different segments).
3. Run transit-stops-file.qmd to create stops.csv in the crayon folder (or create it manually).
  - This script grabs the coordinates of the stops and assigns an ID to each as needed by make_gtfs.
4. Run make_gtfs(MRCagney/make_gtfs).
  - Note that your folder names may differ.
  a. In Windows, open a Terminal in make_gtfs_project (Open the folder in Windows Explorer, right click on empty space and select Open in Terminal)
  b. In the Terminal (Windows PowerShell), type: .\mike_gtfs\Scripts\Activate.ps1 to run python (note your path may be different)
  c. In the Terminal (mike_gtfs), type: python -m make_gtfs "C:\<this_crayon>" "C:\<this_crayon-gtfs>"
  d. C:\<this_crayon-gtfs> is a folder that will be created by make_gtfs that will contain the gtfs files. Keep the quotation marks exactly as shown in the previous line.
  e. In the Terminal (mike_gtfs), type: deactivate
5. Check the gtfs stop_times file.
  - Open the file stop_times that was hopefully created by make_gtfs
    o This file shows all the stops that each run along your new lines will make
  - Open the file stops.csv that you created the gtfs from
    o This file shows the IDs of all the stops
  - Check that each line has as many stops as you expect it to
  - Check that each line runs in both directions
6. Copy the gtfs for the network you want to assess to crayonest/dep/active.
  - This may contain at least several different gtfs feeds
  - This should contain *.osm.pbf, .osm.pbf.mapdb, and .osm.pbf.mapdb.p files that describe the street and walking networks for the region of interest
    o These files are too large to share easily, but can be pulled from openstreetmaps
7. Assess the network.
  a. In assess_crayon.qmd, change crayon_name to the folder name for your crayon
  b. Run assess_crayon.qmd
8. Compare the crayon to an existing network (that has already been assessed).
  a. In compare_crayon.qmd, 
    o Change crayon to the folder name for your crayon.
    o Change crayon_labels to a collection of the lines in your crayon like c("Line 1", "Line 2").
    o Change crayon_breaks to a collection of the lines as named in the shape_id field in your shapes.geojson file like c("line_1, "line_2") in the same order as labels.
    o Change crayon_values to a collection of colours like c("purple", "blue") in the same order as labels.
  b. Run compare_crayon.qmd
9. Optionally, compare the crayon to an alterative network (that has already been assessed).
  a. In compare_alternate_crayons.qmd, 
    o Change crayon1 and crayon2 to the folder names for your crayons.
    o Change crayon_labels1 and crayon_labels2 to a collection of the lines in your crayon like c("Line 1", "Line 2").
    o Change crayon_breaks1 and crayon_breaks2 to a collection of the lines as named in the shape_id field in your shapes.geojson file like c("line_1, "line_2") in the same order as labels.
    o Change crayon_values1 and crayon_values2 to a collection of colours like c("purple", "blue") in the same order as labels.
  b. Run compare_alternate_crayons.qmd
9. Summarize the crayons you have created
  a. In summarize_crayons.qmd, 
    - Change crayon1, crayon2, and crayon3 to the folder names for your crayons.
  b. Run summarize_crayons.qmd


