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
