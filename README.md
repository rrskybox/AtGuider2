# AtGuider2
Automated Autoguide Calibration application for TheSkyX

AtGuider2 Reference (V1.0)

Rick McAlister  09/19/2018

Overview:

AtGuider2 is a Windows console app that automates the TheSkyX autoguider calibration process.  Before running AtGuider2, a user must have TheSkyX configured with imager and guider FOVI’s, and have successfully performed automated image linking (e.g. T-Point runs).  Once launched, ATGuider2 will find an appropriate star near the current pointing position, center the guider camera on the star, adjust the exposure, then calibrate.  AtGuider2 can run with or without a rotator.  AtGuider2 does not calibrate an AO, but it leaves TSX in a state where one could easily do so.
 
Description:

AtGuider2 performs the following series of actions:

1.	Plate solves the current telescope position for position and position angle.

2.	Generates a list of brighter, nearby stars and picks one that is sufficiently isolated from its neighbors that calibration movements will not pick another one up.

3.	Slews the mount to center the targeted star.

4.	Computes an offset position that will center the target star in the guider FOV.

5.	Slews the mount to that offset position and plate solves to update the star chart.

6.	Takes a series of guider images to adjust the targeted star ADU to around 20K, if possible.

7.	Runs autoguider calibration.
 
Usage Notes and Errata:

1.	As of Rev 1.0, AtGuider2 is not particularly robust:  a failure at any stage will either be ignored or throw an error and exit.  See notes below on avoiding problems.

2.	AtGuider2 relies on having the Camera and Guider FOVI’s (Field of View Indicator) set up correctly and accurately.  The TSX forums contain lots of tips to get it right.  As AtGuider2 uses plate solutions (image linking) to position the guider FOV over a targeted star, incorrectly configured FOVI’s will mess the whole thing up.

3.	AtGuider2 reads FOVI information from the Software Bisque “My equipment.txt” file in the “Field of View Indicators” directory.  TSX uses configuration information from this file upon launch, but it does not update any changes until the TSX session is ended.  Therefore, upon creating or modifying any FOVI, the user must close TSX then relaunch in order that AtGuider2 can acquire any FOVI updates, including changes as to the active FOVI selection.

4.	When creating an FOVI for AtGuider2 to use, the main camera FOVI must be “Element 1” and the guider camera FOVI must be “Element 2”.  At2Guider doesn’t care what you name them.  Before running AtGuider2, the FOVI to be used must be the sole FOVI that is selected as active in the FOVI list.  Properly done, only the FOVI of the imager/guider combination should show on the star chart.  (See Note 3).

5.	AtGuider2 uses only the FOVI’s and plate solving (image linking and closed loop slews).  As such, the use of a rotator doesn’t matter, that is, if the FOVI’s are configured correctly.  

6.	AtGuider2 does not subframe for calibration.  It’s not too difficult, but hasn’t been needed, so far.  If it becomes an issue, then maybe in a later version.

7.	AtGuider2 does not mess with the binning on the guider camera.  Whatever the user set will stick.  

8.	If TSX cannot perform an automated image-link then the procedure will fail.  However, if TSX can run T-Point, then AtGuider2 should work fine as the settings are the same.

9.	To automatically find target stars, AtGuider2 uses the same built-in Observing List search as the @Focus2 function.  This simplifies the installation process a bit, but it means that the selected stars might be a touch too bright for optimal calibration (i.e. saturated).  Also, if the telescope is pointing too close to the meridian, target stars could be selected that require a meridian flip.  Solutions to both these issues (as well as AO support) may be incorporated in the future, depending on interest.

10.	AtGuider2 has been tested with the TSX simulators, and on a non-rotating and on a rotating imaging stack.  The latter were mounted on a MyT and MX+ respectively and used Direct drive for guiding through off-axis guide cameras (STX and STI).  Things could be different on different systems – SB simulators, for instance, are oblivious to side-of-pier geometries.  I’ve done my best to note those things that must be configured but I certainly could have missed something important that I insentiently configure on my systems but others may not.  Me culpa prae.

11.	In this version, AtGuider2 does not try to adjust any of calibration set-up parameters.  If calibration is failing because of insufficient movement, then increase the calibration time/distance in the set-up (hot pixels should rarely, if ever be a problem when targeting stars this bright).  If the calibration is failing because of too much movement, then cut down on the calibration time/distance.  

12.	In the telescope set up, “Always keep telescope crosshairs visible on star chart” must be set to “Yes”.  If not, the star selection search might not be so local.

13.	For the FOVI to position correctly on the star chart, the option “Rotate FOVI’s to computed position angle” must be checked.  This option is found at: Tools menu -> Image Link -> FOVI Options”.  In addition, if using a rotator, the “Link FOV to Rotator” configuration must be set in the FOVI set up.

Requirements: 

AtGuider2 is an uncertified Windows console application, written in C#.  The application has been tested under Windows 10 and should, in theory, run under Windows 7 and 8.

Installation:  

Download the "publish" directory. Run the "Setup.exe".  Upon completion, an application icon will have been added to the start menu under "TSXToolKit" with the name "AtGuider2".  This application can be pinned to the Start if desired.

Source:

The unlicensed source code for this application is available on GitHub at “rrskybox/AtGuider2” supported by math library (unlicensed) source code that is available on GitHub at “rrskybox/AstroMath”.

Support:  

This application was written for the public domain and as such is unsupported. The developer wishes you his best and hopes everything works out.  If you find a problem or want to suggest additional features, please contact the author and he'll see what he can do.
