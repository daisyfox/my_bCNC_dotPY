# my_bCNC_dotPY


my_bCNC_dotPY is the result of making amendments to bCNC.py downloaded on 20 Jun 2015.

The files contained here are:

bCNC_orig.py - a copy of the file bCNC.py as downloaded

bCNC.py - the amended file (orig name kept to ensure it worked)

my_bCNC.odt - an OpenOffice colour formatted documentation of my_bCNC.py with background highlighting to identify the changed/added code


The amendments were made to provide a quick fix for a gcode checker based on responses from grbl.  It is simplistic and can be vastly improved upon.

Note: if grbl issued multi-line responses to a gcode block it would corrupt method used by the checker.  Checker relies on a one to one relationship between sent gcode blocks and grbl responses.


POSSIBLE FUTURE DEVELOPMENT & OTHER THOUGHTS:

gcode blocks causing 'error' can be difficult to spot, particularly in long gcode files.  As the vast majority of lines in a gcode file are likely to be 'ok', they could be skipped (when 'run()' is executed while in checkMode).  As long as the user has some way of finding the error causing gcode block in Editor to make corrections (eg using "Find" in the Editor?)

The responses from grbl already available in Terminal are not immediately followed by the corresponding response due to the buffer within grbl.  A 'sent blocks buffer' within bCNC.py may be one way to hold onto sent blocks so that Terminal is only updated with paired sent/rec'd items once the corresponding 'ok'/'error' has been received (when 'run()' is executed while in checkMode but not necessarily liveMode?).

Since failure part way through a running job could be costly (time/materials) Checker should always be run (not just if user selects) and 'live run' should be enabled only when a 'no errors' check has been completed.

it does not appear to be possible to interrogate grbl to find out if it is currently in live or check mode, as the user is able to issue $C command manually it is necessary to keep track of the mode in bCNC.py


SUMMARY OF CHANGES (extract from my_bCNC.odt):

Checker Tab added (def __init__ pg 5)

checkedGcode and checkMode variables added (def __init__ pg 7)

checkGode frame and button added (def __init__ pg 14)

checkertab added (def _checkerTab pg 21)

checkMode flag set if $C issued via Command line (def commandExecute pg 39)

def checkGcode added to so checking occurs (pg 53) 2 step approach needed so that Grbl response to $C didn't 'contaminate' snt/resp pairing compilation

steps in def initRun() (pg 59)  removed (more contamination – better solution req'd)

'if checkMode' code added to def runEnded (pg 61) to update checker text

'if checkMode' code added to def serialIO() (pg 62) so that checking continues to end of gcode file (default is to stoprun on error)

'if checkMode' code added to def monitorSerial() (pg 63) to capture gcode snt and grbl resp


some additional changes were made (mostly on Control tab) to fit the gui to our letterbox screen.