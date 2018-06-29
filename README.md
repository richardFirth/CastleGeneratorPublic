# CastleGenerator
Generates a castle

-------------------------------------------
#Directions for use:
Clone the entire repo to a folder on your computer.
Open the excel file. There will be a yellow bar at the top of the screen asking for permission to run macro's. if you trust me, press the yellow button to grant permission to run macros. (It is dangerous to run macro's you don't know, as they can run malicious code)

There is a button that says **"Spawn Castle"** Press the button. you should get an LDR file on your desktop containing a lego castle. currently it will be in the general style and layout of either King's Castle (70404-1)(blue one in 2013) or King's Castle (7946-1)(Red one in 2011)


##Full description for use by people wishing to alter/modify


**The code is still messy and over-complicated, with certain exeptions that are hardcoded, and objects that do weird stuff, global variables, object names that are weird, etc.**

----------------------------------------
###Build Sections
- Build sections represent an assembly that goes in the castle. this could be something like a wall, tower, gate, roof, tree, or other recognizable section of a castle. These are spawned from the SpawnInstructions. All build sections have their main assembly named "main.ldr". if there is the note "0 SCMD_TABLE" in the build section, then it means that the build section is a table, and one of the subassemblies will be selected when spawned from. See 8x8_Sloped_Roof_Table as a good example of a table build section. The intention is that all the things in the table have the same dimetions in any places that can mate with other parts


----------------------------------------
###Components
- components represent a single thing, such as a flag, section of wall, torch, pillar. Components are used within build sections. the idea of components is to make all of a certain thing behave in the same way (For example, all flags of the castle should be the same colors)
- build sections can be updated to contain the latest version of each component. 


-----------------------------------------
####Commands
- each BuildSection contains commands that are used by the Macro to determine it's final colors and elements. The commands are as follows:

0 REPLACE "Table" ALL: Replace this entire subassembly with one of the subassemblies from the table

0 REPLACE_ID ID "ID1,ID2,ID3" (Key) ALL : Replace each brick  with a given ID with one of the alternate IDS

0 REPLACE_BRICK ID "Table" (Key) ALL : Replace each of a brick with a set of bricks from a table

0 RNDCOLOR 10 "1,2,3,4,5,6" (key) ALL : Replace each element of color 10 with one of the alternate colors

0 REPCOLOR 10 "colorID" : replace each element of color 10 with the color held in colorID


The last item in the command will either be "ALL/ONE" if it's "ALL", then the random selection is the same in all cases. if it's "ONE", then the random selection will be different for each case. 

The (Key) is used to make the random selection the same in all cases with the same key. for example, if we want all the battlements to have the same style of brick, we can use a key to sync this.


-----------------------------------------
###Config
This contains various configuration settings that the castle generator needs to operate (such as a list of all the color definitions)

-----------------------------------------
###Layouts
This contains layouts that are used by the castle spawner to process into a castle. the ultimate goal is to make these layouts generated randomly rather than manually.


-----------------------------------------
###Lookups
When other Components are directed to do a lookup replacement, the lookup table is found in the lookups folder.

-----------------------------------------
###SpawnInstruction
This contains instructions that will spawn a partiular 'structure' as part of the castle. The syntax is as follows

**10x10_Extended_OctTower.ldr@A_rot0, 0.0, 0.0, 0.0**
[BuildSection]@[Rotation],X(bricks),Y(plates),Z(bricks)
spawn the buildsection at the given location. Rotation is an enum and the options are [A_rot0,B_rot90,C_rot180,D_rot270]

**%A,B**
This will choose either ":A" or ":B" and go to that section.

**:A**
Denotes section A

**$OctTowerTop.txt@A_rot0, 0, 38, 0**
The dollar sign tells it to spawn the next element from another set of spawnInstructions (OctTowerTop.txt)

**!8x8_TowerSections@A_rot0, 0, 38, 0**
The exclamation mark tells it to spawn the above section facting 'forward' this is so that different towers in the castle all have their flags facing the right direction.


####Example:	
~~~~
10x10_Extended_OctTower.ldr@A_rot0, 0, 0, 0
%A,A,B
:A
DrumTowerLvL2.ldr@A_rot0, 0, 19, 0
$OctTowerTop.txt@A_rot0, 0, 38, 0
:B
10x10_Oct_Battlement_Corner_Table.ldr@A_rot0, 0, 19, 0
~~~~~~~~ 

####Second Example:
~~~~~~~~ 
8x8_TowerBase@A_rot0, 0, 0, 0
!8x8_TowerUpper@A_rot0, 0, 19, 0
%A,B
:A
!8x8_TowerSections@A_rot0, 0, 38, 0
!8x8_TowerSections@A_rot0, 0, 60, 0
!8x8_Sloped_Roof_Table@A_rot0, 0, 82, 0
:B
!8x8_TowerSections@A_rot0, 0, 38, 0
!8x8_TowerSections@A_rot0, 0, 60, 0
8_to_10@A_rot0, 0, 82, 0
!10x10_Crennelation@A_rot0, 0, 86, 0
~~~~~~~~ 











