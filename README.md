# WindowStation
Secure alternate desktop shell with taskbar, start menu, and gamepad support.

To disconnect xinput to apps use WTSDisconnect.exe.  You can navigate app Windows with the mouse rather than the silly selector rectangle.

Code:

```
Const WTS_CURRENT_SESSION       As Long = -1
Const WTS_CURRENT_SERVER_HANDLE As Long = 0
Private Declare Function apiWTSDisconnectSession Lib "wtsapi32" Alias "WTSDisconnectSession" (ByVal hServer As Long, ByVal SessionID As Long, ByVal bWait As Boolean) As Boolean
Private Declare Function apiWTSOpenServer Lib "wtsapi32" Alias "WTSOpenServer" (ByVal pServerName As String) As Long
Private Declare Function apiWTSCloseServer Lib "wtsapi32" Alias "WTSCloseServer" (ByVal hServer As Long) As Boolean
Sub main()
   DissconnectSession
End Sub
Private Function DissconnectSession(Optional ByVal ServerName As String, Optional ByVal SessionID As Long) As Boolean
   On Error Resume Next
   Dim OpenedServer As Long
   OpenedServer = WTS_CURRENT_SERVER_HANDLE
   If ServerName <> "" Then OpenedServer = apiWTSOpenServer(ServerName)
   If SessionID = 0 Then SessionID = WTS_CURRENT_SESSION
   DissconnectSession = apiWTSDisconnectSession(OpenedServer, SessionID, False)
   Call apiWTSCloseServer(OpenedServer)
End Function
```

You can configure the gamepad driver to work on the Winlogon desktop at boot and at UAC prompts.

Registry editor code:
```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Accessibility\ATs\xinp]
"ApplicationName"=hex(2):57,00,69,00,6e,00,64,00,6f,00,77,00,47,00,61,00,6d,00,\
  65,00,70,00,61,00,64,00,00,00
"ATExe"="WindowGamepad.exe"
"CopySettingsToLockedDesktop"=dword:00000001
"Description"="Xinput Mouse"
"Profile"="<HCIModel><Accommodation type=\"severe dexterity\" /><Accommodation type=\"mild dexterity\" /></HCIModel>"
"SimpleProfile"="xinp"
"StartExe"=hex(2):45,00,3a,00,5c,00,57,00,69,00,6e,00,64,00,6f,00,77,00,53,00,\
  74,00,61,00,74,00,69,00,6f,00,6e,00,5c,00,57,00,69,00,6e,00,64,00,6f,00,77,\
  00,4c,00,61,00,75,00,6e,00,63,00,68,00,65,00,72,00,5c,00,57,00,69,00,6e,00,\
  64,00,6f,00,77,00,47,00,61,00,6d,00,65,00,70,00,61,00,64,00,2e,00,65,00,78,\
  00,65,00,00,00
"TerminateOnDesktopSwitch"=dword:00000000
"StartParams"="/uac"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Accessibility]
"Configuration"="xinp"
```

Be sure to edit the registry value for StartExe, after you import it into the registry, ie update the registry string rather than the hex.

To uninstall from boot/uac:
```
Windows Registry Editor Version 5.00
[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Accessibility\ATs\xinp]
```
