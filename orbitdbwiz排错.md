## 排错指令
set "ODB=%USERPROFILE%\orbitdbwiz.exe"

echo.
echo ==================== 1. FILE PATH ====================
echo %ODB%

echo.
echo ==================== 2. FILE EXISTS ====================
dir /a "%ODB%" 2>&1

echo.
echo ==================== 3. FILE PERMISSIONS ====================
icacls "%ODB%" 2>&1

echo.
echo ==================== 4. FILE INFORMATION ====================
powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB | Format-List FullName,Length,CreationTime,LastWriteTime,Attributes" 2>&1

echo.
echo ==================== 5. FILE STREAMS ====================
powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize" 2>&1

echo.
echo ==================== 6. DIGITAL SIGNATURE ====================
powershell -NoProfile -Command "Get-AuthenticodeSignature -LiteralPath $env:ODB | Format-List Status,StatusMessage,Path" 2>&1

echo.
echo ==================== 7. SHA256 ====================
certutil -hashfile "%ODB%" SHA256 2>&1

echo.
echo ==================== 8. RUN ORBITDBWIZ HELP ====================
"%ODB%" --help 2>&1

echo.
echo ==================== 9. APPLOCKER LOG ====================
wevtutil qe "Microsoft-Windows-AppLocker/EXE and DLL" /rd:true /f:text /c:50 2>&1 | findstr /i /c:"orbitdbwiz" /c:"denied" /c:"blocked" /c:"prevented"

echo.
echo ==================== 10. CODE INTEGRITY LOG ====================
wevtutil qe "Microsoft-Windows-CodeIntegrity/Operational" /rd:true /f:text /c:100 2>&1 | findstr /i /c:"orbitdbwiz" /c:"denied" /c:"blocked" /c:"prevented"

echo.
echo ==================== FINISHED ====================

#输出：
Microsoft Windows [Version 10.0.22631.6936]
(c) Microsoft Corporation. All rights reserved.

C:\Users\QXZ4Y4A>set "ODB=%USERPROFILE%\orbitdbwiz.exe"

C:\Users\QXZ4Y4A>echo %ODB%
C:\Users\QXZ4Y4A\orbitdbwiz.exe

C:\Users\QXZ4Y4A> icacls "%ODB%" 2>&1
C:\Users\QXZ4Y4A\orbitdbwiz.exe NT AUTHORITY\SYSTEM:(F)
                                BUILTIN\Administrators:(F)
                                CHINA\QXZ4Y4A:(F)

Successfully processed 1 files; Failed processing 0 files

C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB | Format-List FullName,Length,CreationTime,LastWriteTime,Attributes" 2>&1


FullName      : C:\Users\QXZ4Y4A\orbitdbwiz.exe
Length        : 14664192
CreationTime  : 2026/7/21 13:41:32
LastWriteTime : 2026/7/21 13:43:53
Attributes    : Archive




C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize" 2>&1

Stream            Length
------            ------
:$DATA          14664192
Zone.Identifier      112



C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Get-AuthenticodeSignature -LiteralPath $env:ODB | Format-List Status,StatusMessage,Path" 2>&1


Status        : NotSigned
StatusMessage : The file C:\Users\QXZ4Y4A\orbitdbwiz.exe is not digitally signed. You cannot run this script on the cur
                rent system. For more information about running scripts and setting execution policy, see about_Executi
                on_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170
Path          : C:\Users\QXZ4Y4A\orbitdbwiz.exe




C:\Users\QXZ4Y4A>certutil -hashfile "%ODB%" SHA256 2>&1
SHA256 hash of C:\Users\QXZ4Y4A\orbitdbwiz.exe:
d9286934c01a484fe267572d5e24e1996be8de1624f1cc38af486492e27c78dd
CertUtil: -hashfile command completed successfully.

C:\Users\QXZ4Y4A>"%ODB%" --help 2>&1
Access is denied.

C:\Users\QXZ4Y4A>wevtutil qe "Microsoft-Windows-AppLocker/EXE and DLL" /rd:true /f:text /c:50 2>&1 | findstr /i /c:"orbitdbwiz" /c:"denied" /c:"blocked" /c:"prevented"
%SYSTEM32%\NLAAPI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\ONDEMANDCONNROUTEHELPER.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINNSI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NETSETUPENGINE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINNSI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NETSETUPENGINE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINNSI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NETSETUPENGINE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINNSI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NETSETUPENGINE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NTDSAPI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%WINDIR%\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.22621.6931_NONE_270FD4B77385A2C0\COMCTL32.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSROLE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%WINDIR%\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.22621.6931_NONE_270FD4B77385A2C0\COMCTL32.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\AUTHZ.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\FRAMEDYNOS.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSUIEXT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSSEC.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\GPEDIT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%WINDIR%\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.22621.6931_NONE_270FD4B77385A2C0\COMCTL32.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%WINDIR%\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.22621.6931_NONE_270FD4B77385A2C0\COMCTL32.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\AUTHZ.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\NTDSAPI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSROLE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\FRAMEDYNOS.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSUIEXT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DSSEC.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\GPEDIT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\AVRT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\MFSENSORGROUP.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\FRAMESERVERCLIENT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\COMPPKGSUP.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\MFCORE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\MFCAPTUREENGINE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\IGC64.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\IGDGMM64.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\IGDGMM2_64.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\INTELCONTROLLIB.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINMM.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\IGD10UM64XE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DRIVERSTORE\FILEREPOSITORY\IIGD_DCH.INF_AMD64_0EAC281DC2D07A5F\IGD10IUMD64.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DIRECTXDATABASEHELPER.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\RESOURCEPOLICYCLIENT.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DXCORE.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DXGI.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\D3D11.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\WINDOWS.MEDIA.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.
%SYSTEM32%\DEFAULTDEVICEMANAGER.DLL was allowed to run but would have been prevented from running if the AppLocker policy were enforced.

C:\Users\QXZ4Y4A>wevtutil qe "Microsoft-Windows-CodeIntegrity/Operational" /rd:true /f:text /c:100 2>&1 | findstr /i /c:"orbitdbwiz" /c:"denied" /c:"blocked" /c:"prevented"

C:\Users\QXZ4Y4A>


输入：
set "ODB=%USERPROFILE%\orbitdbwiz.exe"

echo ==================== BEFORE ====================
powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize"

echo.
echo ==================== REMOVE DOWNLOAD BLOCK ====================
powershell -NoProfile -Command "Unblock-File -LiteralPath $env:ODB"

echo.
echo ==================== AFTER ====================
powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize"

echo.
echo ==================== RUN TEST ====================
"%ODB%" --help

echo.
echo ==================== EXIT CODE ====================
echo %ERRORLEVEL%

输出
C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize"

Stream            Length
------            ------
:$DATA          14664192
Zone.Identifier      112



C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Unblock-File -LiteralPath $env:ODB"

C:\Users\QXZ4Y4A>powershell -NoProfile -Command "Get-Item -LiteralPath $env:ODB -Stream * | Format-Table Stream,Length -AutoSize"

Stream   Length
------   ------
:$DATA 14664192



C:\Users\QXZ4Y4A>"%ODB%" --help
Access is denied.

C:\Users\QXZ4Y4A>echo %ERRORLEVEL%
5

C:\Users\QXZ4Y4A>
