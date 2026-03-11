Sub Main()
' declare character and numeric string variables and their variable types before using them, this is
done with the Dimension statement
Dim ConfirmReady As String
Dim DoXY As String
Dim DoZ As String
Dim XaxisDRO As Integer
Dim YaxisDRO As Integer
Dim ZaxisDRO As Integer
' define variable values used by the SetOEMDRO() command
XaxisDRO=800
YaxisDRO=801
ZaxisDRO=802
' the next line makes sure the operator has the touch plate leads connected and is ready, otherwise
exit completely
ConfirmReady = AskTextQuestion("Confirm Touch plate leads are connected and ready. (y/n)")
' if the response is yes then jump to 1: otherwise jump to 5:
If ConfirmReady = "y" Then GoTo 1 Else GoTo 5
1:
' present the option to zero X & Y or bypass and go straight to zeroing Z
DoXY = AskTextQuestion("Zero X and Y also? (y/n)")
' if the response is yes then jump to 2: and begin XY “touching” otherwise jump to 3: and begin
with Z “touching
If DoXY = "y" Then GoTo 2 Else GoTo 3
2:
' write message to Mach3's status line
Message( "Auto Zeroing X..." )
' zero the x-axis where ever it is to ensure it moves toward the touch plate
SetOEMDRO(XaxisDRO, 0.0000)
' give the command time to “zero” the DRO and let the CPU do other things for 1000
milliseconds
Sleep 1000
If IsSuchSignal (22) Then ' signal 22 equates to the Input 15 port of the BOB
' if the touch plate leads have not touched yet move the X-axis toward the value of -2 with a feed
rate of 10
code "G31 X-2 F10"
' while the x-axis is moving let the CPU do other things for 100 milliseconds until the touch plate
input goes low
While IsMoving()
Sleep 100
' this is the end to the While IsMoving statement
Wend
' at the point the touch plate leads touch, set the X-axis DRO to half the diameter of the tool bit.
' if you are using a 1/4” shank bit change the .0625 to .125, make the same change in the section
for the Y-axis
SetOEMDRO(XaxisDRO, .0625)
' let the CPU do other things while this is happening
Sleep 1000
' then move the tool in the X-axis away from the touch plate to +.5”
code "G1 X.5"
' we are done with zeroing the X-axis proceed immediately to do Y
End If
Message( "Auto Zeroing Y..." )
SetOEMDRO(YaxisDRO, 0.0000)
Sleep 1000
If IsSuchSignal (22) Then
code "G31 Y-1 F10"
While IsMoving()
Sleep 100
Wend
SetOEMDRO(YaxisDRO, .0625)
Sleep 1000
code "G1 Y.5"
End If
3:
' get a confirmation that the user wants to proceed with the Z zeroing
DoZ = AskTextQuestion("Position the touch plate to zero Z. y to continue or n to skip. (y/n)")
If DoZ = "y" Then GoTo 4 Else GoTo 6
4:
Message( "Auto Zeroing Z..." )
SetOEMDRO(ZaxisDRO, 0.0000)
Sleep 1000
If IsSuchSignal (22) Then
code "G31 Z-2 F10"
While IsMoving()
Sleep 100
Wend
' the thickness of the z touch plate described in this document is .180 inches
Call SetOEMDRO(ZaxisDRO, .180)
Sleep 100
code "G1 Z1"
End If
GoTo 6
5:
Message ("Tool zeroing aborted. Try again when ready.")
GoTo 7
6:
Message "Tool zeroing complete. Check the results on the DROs."
7:
End Sub
' VB syntax defines a subroutine starting with “Sub 'name'()” line, e.g. Sub Main() as the
beginning point of a VB
' subroutine and defines the ending of a subroutine with “End Sub”
' In a similar fashion an “If...Then” statement must have a closing “End If” statement unless the
“If...Then
' performs an immediate “GoTo” jump command.
