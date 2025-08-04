# JLCPCB KiCad Integration Example

This repository is a two layer 5V DC-DC power supply, created in KiCad and designed to integrate with JLCPCB. The power supply was just a test board I created, it should not be used for anything meaningful, but you could use this project as a template for new projects or just to test submitting to JCLPCB, and just delete the board unordered when you are done.

## Installing Libraries/Plugins

To get your local KiCad installation setup properly, open the Plugin and Content Manager.

1. Install the 'Fabrication Toolkit' Plugin from the KiCad official repository.

2. Install the 'impartGUI' Plugin from the KiCad official repository. Yes, that likely should be 'import GUI', but it's not.

3. In the Plugin and Content Manager, click the 'Manage...' button and add a new Repository with the URL: https://raw.githubusercontent.com/CDFER/cd_fer-kicad-repository/main/repository.json

4. Select 'CD_FER's KiCad repository' from the Repository drop down, select the 'Library' tab below, and then install 'JLCPCB KiCad Library'. Then 'Apply Changes'.

## Using 'JLCPCB KiCad Library'

'JLCPCB KiCad Library' integrates itself into KiCad as provider of 3rd party symbols and matching footprints for all of the JLCPCB basic and preferred parts and for some selected popular extended parts. Basic parts are the parts that don't cost extra month for 'Economic' PCB assembly. To use this library:

1. Open the schematic editor and select 'Place | Place Symbols'

2. Search for a part, for example '100nf', '10kΩ', 'jlcpcb schottky'. If you want to browse all of the JLCPCB parts search for 'jlcpcb'

3. Select a matching part from the 'PCM_JLCPCB\*' folder. The name of the folder will vary based on the type of part.

I tend to use this mostly for passive resistors and capacitors and use impartGUI for everything else.

## impartGUI Configuration

If you use impartGUI to import LCSC part numbers (LCSC is the JLCPCB part supplier), you will need to tell KiCad where to find the footprints and symbols that impartGUI manages.

1. From the schematic editor, select 'Preferences | Manage Symbol Libraries' and add either a Global or Project Specific library that points to the .kicad_sym file that impart GUI downloaded. For example, in this project, it's a project specific directory, with a path ${KIPRJMOD}/LCSC/EasyEDA.kicad_sym.

2. From the PCB editor, select 'Preferences | Manage Footprint Libraries' and add either a Global or a Project Specific Library that points to the EasyEDA.pretty directory that impart GUI manages. In this project it's ${KIPRJMOD}/LCSC/EasyEDA.pretty

3. Make sure the nickname of the Footprint library is 'EasyEDA', if it's not, integration between symbols and footprints will break.

If you use JLCPCB parts across more than one project and reuse the same parts, I'd recommend using a Global symbol and Global Footprint library.

## Using impart GUI

1. You access impartGUI from the PCB editor, using the 'Tools | External Plugins | impartGUI' menu.

2. You can find LCSC part numbers on the [LCSC Website](https://lcsc.com). Also give this new website a try: [LCSC Parts Search Tool](https://lcscpartsearchgit.streamlit.app/?embed_options=dark_theme). The Part number should start with a 'C', for example, 'C109322'.

3. Enter the part number in text box to the right of the 'Manual Import' button, select 'auto KiCad settings', and make sure your 'Library Save Location' matches the root directory of the footprint and symbol library configuration you provided earlier. This doesn't except any of the standard KiCad locations like ${KIPRJMOD}, so you will have to enter the whole path, for example /Users/myuser/Documents/Project/LCSC.

4. Then click 'Manual Import'. If you don't see any errors, it should tell you all of the files it downloaded and where it put them.

### If you see an error when running impartGUI

impart GUI can fail to install properly, it complained that the python library, easyeda2kicad was not installed when I first ran it.

Fixing missing python libraries on MacOS:

1. Find the KiCad.app, usually /Applications/KiCad/KiCad.app, but if different for yourself, note the difference and adjust the next step.
2. Launch a terminal window and issue the command:

    ```
    cd /Applications/KiCad/KiCad.app/Contents/Frameworks/Python.framework/Versions/Current/bin
    ```

3. Install the missing library
    ```
    ./pip3 install easyeda2kicad
    ```

### If 3D models are missing in the 3D view

If parts you downloaded using impartGUI are missing 3D models in the 3D view, select the part in the PCB editor, hit the 'e' key to edit, click on 3D models, edit the path, it should point to the EasyEDA.3dshapes that impartGUI manages. You can hard code the path, but it's recommended to use the project environmental variables like `${KIPRJMOD}` or `${KICAD9_3RD_PARTY}`.

## Using 'Fabrication Toolkit'

1. Complete your KiCad design, ensuring that all DRC checks pass and from the PCB editor select 'Tools | External Plugins | Fabrication Toolkit'

2. You are likely fine using the default options. Just click 'Generate'. This will produce a 'production' folder in your project directory.

3. Go to JLCPCB and click 'Order now' from their homepage.

4. Click 'Add Gerber File' and select the Project.zip file (it will be named for your project name) from your 'production' folder.

5. Wait for JLCPCB to process your gerber files and review the options. I usually accept all the defaults. Scroll down to 'PCB Assembly' and turn that option on. Make sure 'Economic' is available and selected. If your board is simple and small, it should be. If it's not it's likely you selected a more complex option from the list above, or just have a very complex board not eligible for economic assembly.

6. Click the 'Next' button on the right. From the top of the screen select the 'BOM' tab. Click 'Add BOM File' and upload the bom.csv file from the production directory.

7. Click 'Add CPL file' and upload the positions.csv file from the production directory.

8. Click 'Process BOM & CPL'

9. Review the BOM. Correct any errors. All of the parts should be found. If you have an unmatched component you can search for a replacement in the LCSC inventory, or go back to your project in KiCad to research what went wrong. Click 'Next' when you are done. If you still have unmatched parts you will be asked to confirm that you don't wish to place them (maybe you will hand solder a part you already have)

10. Review the PCB layout carefully paying specific attention to rotation. Click on every part on the right hand side list and review it in the 3D preview. Ensure it's rotated properly and positioned properly. ICs, connectors and polarized parts tend to have issues here. Hit the space bar to rotate a part, the arrow keys nudge a misaligned part. When you are done, click 'Next'.

11. Enter a Project description and save to cart.

You did it!
