# WindowStation
Secure alternate desktop shell with taskbar, start menu, and gamepad support.


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
"ApplicationName"=hex(2):40,00,25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,\
  6f,00,6f,00,74,00,25,00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,\
  00,5c,00,41,00,63,00,63,00,65,00,73,00,73,00,69,00,62,00,69,00,6c,00,69,00,\
  74,00,79,00,43,00,50,00,4c,00,2e,00,64,00,6c,00,6c,00,2c,00,2d,00,38,00,35,\
  00,00,00
"ATExe"="WindowGamepadUAC.exe"
"CopySettingsToLockedDesktop"=dword:00000001
"Description"="Xinput Mouse"
"Profile"="<HCIModel><Accommodation type=\"severe dexterity\" /><Accommodation type=\"mild dexterity\" /></HCIModel>"
"SimpleProfile"="xinp"
"StartExe"=hex(2):00
"TerminateOnDesktopSwitch"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Accessibility\Session1]
"SecureConfiguration"=""
"Configuration"="xinp"
```

Be sure to edit the registry value for StartExe, after you import it into the registry.  Update the registry string rather than the hex.
To uninstall from boot/uac:
```
Windows Registry Editor Version 5.00
[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Accessibility\ATs\xinp]
```
