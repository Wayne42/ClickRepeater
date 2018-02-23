#include <GUIConstantsEx.au3>
#include <GuiListView.au3>

;Click Repeater
;Author: Ferhat Tohidi Far
;Beweggrund: Wollte jemanden im Teamspeak mehrmals anstupsen.
;PixelCheck Funktion sollte verhindern, dass der Bot falsche Leute anstupst. Hat funktioniert.
;Ist der Code lesbar? Ich hoffs doch.
;ferhat064@hotmail.de


Opt("GUIOnEventMode" , 1)


HotKeySet("+1", "addLinks")
HotKeySet("+2", "addRechts")
HotKeySet("+3", "repeat")
HotKeySet("+4", "Pause")
HotKeySet("+5", "resetStack")
HotKeySet("{Esc}", "Quit")


Func Quit()
	  SetStatus("QUIT")
	  GUIDelete()
	  Exit
   EndFunc

;pausehelfer
Global $pause = true
Func Pause()
   if $pause then
		 $pause = false
		 GUICtrlSetBkColor($s4, 0xFF0000)
		 SetStatus("Gestoppt!")

		 Sleep(100)
		 GUICtrlSetBkColor($s4, 0xFFFFFF)
	  EndIf

	  EndFunc
;ende


;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI

$meinGui =  GUICreate("Click Repeater (Stack)", 700, 500)
GUISetBkColor(0xAAAAAA)

;linke Seite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite
;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite
;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite

;statuszeilen bereich
$statusLabel = GUICtrlCreateLabel("Statuszeile:", 10, 10, 390, 20)
GUICtrlSetBkColor($statusLabel, 0x00AAFF)
GUICtrlSetColor($statusLabel, 0x000000)

Global $ctrl_status = GuiCtrlCreateInput("Instruktionen siehe unten!", 10, 40,390, 30)
GUICtrlSetBkColor($ctrl_status, 0x777777)
GUICtrlSetColor($ctrl_status, 0xFFFFFF)
;ende

;instruktionen bereich
$red = GUICtrlCreateLabel("Kontrolltasten: ", 10, 280, 390)
GUICtrlSetBkColor($red, 0x00EEFF)
$s1 = GUICtrlCreateLabel("Shift + 1 um Linksklick auf momentaner Mausposition zu registrieren! ", 10, 300, 390)
$s2 = GUICtrlCreateLabel("Shift + 2 um Rechtsklick auf momentaner Mausposition zu registrieren! ", 10, 320, 390)
$s3 = GUICtrlCreateLabel("Shift + 3 um den Stack abzuspielen. ", 10, 340, 390)
$s4 = GUICtrlCreateLabel("Shift + 4 um den Stack abzubrechen. ", 10, 360, 390)
$s5 = GUICtrlCreateLabel("Momentan sind bis zu 100 Klicks speicherbar!", 10, 380, 390)
$s6 = GUICtrlCreateLabel("Mit der PC Option kann man vor dem Klick durch PixelChecksum", 10, 400, 390)
$s7 = GUICtrlCreateLabel("nach Veränderung im Klickbereich prüfen. Falls Verändert = Kein Klick! ", 10, 420, 390)
GUICtrlSetBkColor($s1, 0xFFFFFF)
GUICtrlSetBkColor($s2, 0xFFFFFF)
GUICtrlSetBkColor($s3, 0xFFFFFF)
GUICtrlSetBkColor($s4, 0xFFFFFF)
GUICtrlSetBkColor($s5, 0x00EEFF)
GUICtrlSetBkColor($s6, 0x00EEFF)
GUICtrlSetBkColor($s7, 0x00EEFF)

;ende

;checker
Global $checker = GUICtrlCreateCheckbox("Pixel Check Toggler (PC)", 10, 450)
Global $enableChecker
Global $pCH[100]
Func pixelCheck()

$pCH[$index] = PixelChecksum(MouseGetPos(0)-3,MouseGetPos(1)-3, MouseGetPos(0)+3, MouseGetPos(1)+3)

EndFunc

Func pixelCheckRunTime()

   MouseMove(($stack[$hilfeRepeat])[0], ($stack[$hilfeRepeat])[1], 1)

$a = PixelChecksum(MouseGetPos(0)-3,MouseGetPos(1)-3, MouseGetPos(0)+3, MouseGetPos(1)+3)

$b = $pCH[$hilfeRepeat]

SetStatus($a & "  " & $b)

if  $a =  $b Then
   return True
Else
   return False
   EndIf

   EndFunc
;ende

;user input bereich
GUICtrlCreateLabel("Wie oft Stack wiederholen?", 10, 80)
Global $wieOft = GuiCtrlCreateInput("1", 10, 100,390, 30)
GUICtrlCreateLabel("Wieviel Delay zwischen Klicks in ms?", 10, 130)
Global $klickDelay = GuiCtrlCreateInput("10", 10, 150,390, 30)
GUICtrlCreateLabel("Wieviel Delay zwischen Wiederholungen in ms?", 10, 180)
Global $wiederholungDelay = GuiCtrlCreateInput("10", 10, 200,390, 30)
;ende

;stack variabeln
Global $stack[100]
Global $index = 0
;ende





;globale hilfsvariabeln
Global $boolean = false
Global $hilfeRepeat = 0

;RESET BUTTON
$resetButton = GUICtrlCreateButton("Reset Stack", 10, 240, 390)
GUICtrlSetBkColor($resetButton, 0x00AAFF)
GUICtrlSetColor($resetButton, 0x000000)
GUICtrlSetOnEvent($resetButton, "resetStack")
Func resetStack()
   SetStatus("Reset started")
   While $index > 0
	  GuiCtrlDelete($labelStack[$index])

	  $index = $index - 1
	  GuiCtrlDelete($labelStack[$index])

   WEnd
   $pos1 = 420
   $pos2 = 50
   GuiCtrlSetState($checker, $GUI_ENABLE)
   SetStatus("Reset Complete")
EndFunc
;ENDE

;linke Seite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite
;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite
;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite;linkeSeite


   ;rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite)
   ;rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite)

;hilfe für stackLabel pos1 x koord, pos2 y koord, labelstack sollte so groß wie das stack array sein
Global $pos1 = 450
Global $pos2 = 40
Global $labelStack[100]
Global $idListview = GUICtrlCreateListView("X Koord | Y Koord | Klick  ", $pos1, $pos2, 240, 402)
;ende

$lab = GUICtrlCreateLabel("Dein Stack:", $pos1, 10, 240)
GUICtrlSetBkColor($lab, 0x00AAFF)
GUICtrlSetColor($lab, 0x000000)
   ;rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite)
   ;rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite);rechte seite (stack seite)


GUISetState(@SW_SHOW)
GUISetOnEvent($GUI_EVENT_CLOSE, "Quit")
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI
;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI;GUI


While 1

 WEnd



;<hilfsfunktionen>

Func SetStatus( $msg )
	GUICtrlSetData( $ctrl_status, $msg )
 EndFunc


;</hilfsfunktionen>


;<eigentlicherBotTeil>
Func addLinks()
   GUICtrlSetBkColor($s1, 0x00AAFF)

    $x = MouseGetPos(0)
    $y = MouseGetPos(1)

   if GUICtrlRead($checker) = $GUI_CHECKED then
	  pixelCheck()
	  $enableChecker = false
   EndIf
   GuiCtrlSetState($checker, $GUI_DISABLE)
	  Sleep(100)

	  Local $internalArray[3] = [$x, $y, 0]
	  $stack[$index] = $internalArray


	  $labelStack[$index] = GUICtrlCreateListViewItem($internalArray[0] & "|" & $internalArray[1] & "|Links"  , $idListview)
	  $index = $index +1
	  SetStatus("X Position " & $internalArray[0] & " Y Position "  & $internalArray[1] & " Linksklick")



	  $pos1 =  $pos1
	  $pos2 =  $pos2 + 20
	  _GUICtrlListView_Scroll($idListview, 0, 20)
	  GUICtrlSetBkColor($s1, 0xFFFFFF)

   EndFunc

Func addRechts()
   GUICtrlSetBkColor($s2, 0x00AAFF)

    $x = MouseGetPos(0)
    $y = MouseGetPos(1)

   if GUICtrlRead($checker) = $GUI_CHECKED then
   pixelCheck()
   $enableChecker = false
EndIf
GuiCtrlSetState($checker, $GUI_DISABLE)

   Sleep(100)

   Local $internalArray[3] = [$x, $y, 1]
   $stack[$index] = $internalArray


   $labelStack[$index] = GUICtrlCreateListViewItem($internalArray[0] & "|" & $internalArray[1] & "|Rechts"  , $idListview)
   $index = $index +1
   SetStatus("X Position " & $internalArray[0] & " Y Position "  & $internalArray[1] & " Rechtsklick")



   $pos1 =  $pos1
   $pos2 =  $pos2 + 20
   _GUICtrlListView_Scroll($idListview, 0, 20)
   GUICtrlSetBkColor($s2, 0xFFFFFF)
EndFunc



Func repeat()

   $pause = true
   GuiCtrlSetState($checker, $GUI_DISABLE)
   GUICtrlSetBkColor($s3, 0xFF0000)
   $hilfeRepeat = 0
   $von = 0
   $bis = GUICtrlRead($wieOft)


   While $von < $bis AND $pause
		 _GUICtrlListView_Scroll($idListview, 0, -2000)
		 While $hilfeRepeat < $index AND $pause

;;PC OPTION AN
		 if GUICtrlRead($checker) = $GUI_CHECKED then

			SetStatus("Klick Nummer " & $hilfeRepeat & " Wiederholung: (" & $von+1 & "/" & $bis & ") ")

					 if ($stack[$hilfeRepeat])[2] = 0 AND pixelCheckRunTime() = True Then
						MouseClick("left", ($stack[$hilfeRepeat])[0], ($stack[$hilfeRepeat])[1], 1, 0)
						GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0x00AAFF)
					 EndIf

					 if ($stack[$hilfeRepeat])[2] = 1 AND pixelCheckRunTime() = true  Then
						MouseClick("right", ($stack[$hilfeRepeat])[0], ($stack[$hilfeRepeat])[1], 1, 0)
						GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0x00AAFF)
					 EndIf

			Sleep(GUICtrlRead($klickDelay))
			GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0xFFFFFF)
			 _GUICtrlListView_Scroll($idListview, 0, 20)
			$hilfeRepeat = $hilfeRepeat + 1

;;PC OPTION NICHT AN

		 Else
			SetStatus("Klick Nummer " & $hilfeRepeat & " Wiederholung: (" & $von+1 & "/" & $bis & ") ")
						if ($stack[$hilfeRepeat])[2] = 0 Then
						MouseClick("left", ($stack[$hilfeRepeat])[0], ($stack[$hilfeRepeat])[1], 1, 0)
						GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0x00AAFF)
					 EndIf

					 if ($stack[$hilfeRepeat])[2] = 1 Then
						MouseClick("right", ($stack[$hilfeRepeat])[0], ($stack[$hilfeRepeat])[1], 1, 0)
						GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0x00AAFF)
					 EndIf

			Sleep(GUICtrlRead($klickDelay))
			GuiCtrlSetBkColor($labelStack[$hilfeRepeat], 0xFFFFFF)
			 _GUICtrlListView_Scroll($idListview, 0, 20)
			$hilfeRepeat = $hilfeRepeat + 1



		 EndIf

		 WEnd


	  $von = $von +1
	  $hilfeRepeat = 0
	  Sleep(GUICtrlRead($wiederholungDelay))

   WEnd

   GUICtrlSetBkColor($s3, 0xFFFFFF)
   if $enableChecker then
   GuiCtrlSetState($checker, $GUI_ENABLE)
   EndIf
   SetStatus("Fertig!")




EndFunc
;</eigentlicherBotTeil>