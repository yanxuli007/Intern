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


输入：
powershell -NoProfile -Command "$odb=\"$env:USERPROFILE\orbitdbwiz.exe\";$test=\"$env:TEMP\orbitdbwiz-test.exe\";Write-Host '===== 1 APPLOCKER POLICY =====';$p=Get-AppLockerPolicy -Effective -ErrorAction SilentlyContinue;if($p){Test-AppLockerPolicy -PolicyObject $p -Path $odb -User \"$env:USERDOMAIN\$env:USERNAME\" | Format-List}else{Write-Host 'Unable to read effective AppLocker policy'};Write-Host '===== 2 COPY TO TEMP =====';Copy-Item -LiteralPath $odb -Destination $test -Force;icacls $test;Write-Host '===== 3 RUN TEMP COPY =====';& $test --help;Write-Host ('Exit code: '+$LASTEXITCODE);Write-Host '===== 4 APPLOCKER EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-AppLocker/EXE and DLL' -MaxEvents 300 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 5 CODE INTEGRITY EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-CodeIntegrity/Operational' -MaxEvents 500 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 6 DEFENDER EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-Windows Defender/Operational' -MaxEvents 500 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz|d9286934c01a484fe267572d5e24e1996be8de1624f1cc38af486492e27c78dd'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 7 SOFTWARE RESTRICTION POLICY =====';reg query 'HKLM\SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers' /s;reg query 'HKCU\SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers' /s;Write-Host '===== 8 APPLICATION EVENTS =====';$since=(Get-Date).AddMinutes(-10);Get-WinEvent -FilterHashtable @{LogName='Application';StartTime=$since} -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz|Access is denied|blocked|prevented|restriction'} | Select-Object TimeCreated,ProviderName,Id,LevelDisplayName,Message | Format-List;Write-Host '===== FINISHED ====='"


输出
C:\Users\QXZ4Y4A>powershell -NoProfile -Command "$odb=\"$env:USERPROFILE\orbitdbwiz.exe\";$test=\"$env:TEMP\orbitdbwiz-test.exe\";Write-Host '===== 1 APPLOCKER POLICY =====';$p=Get-AppLockerPolicy -Effective -ErrorAction SilentlyContinue;if($p){Test-AppLockerPolicy -PolicyObject $p -Path $odb -User \"$env:USERDOMAIN\$env:USERNAME\" | Format-List}else{Write-Host 'Unable to read effective AppLocker policy'};Write-Host '===== 2 COPY TO TEMP =====';Copy-Item -LiteralPath $odb -Destination $test -Force;icacls $test;Write-Host '===== 3 RUN TEMP COPY =====';& $test --help;Write-Host ('Exit code: '+$LASTEXITCODE);Write-Host '===== 4 APPLOCKER EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-AppLocker/EXE and DLL' -MaxEvents 300 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 5 CODE INTEGRITY EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-CodeIntegrity/Operational' -MaxEvents 500 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 6 DEFENDER EVENTS =====';Get-WinEvent -LogName 'Microsoft-Windows-Windows Defender/Operational' -MaxEvents 500 -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz|d9286934c01a484fe267572d5e24e1996be8de1624f1cc38af486492e27c78dd'} | Select-Object TimeCreated,Id,LevelDisplayName,Message | Format-List;Write-Host '===== 7 SOFTWARE RESTRICTION POLICY =====';reg query 'HKLM\SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers' /s;reg query 'HKCU\SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers' /s;Write-Host '===== 8 APPLICATION EVENTS =====';$since=(Get-Date).AddMinutes(-10);Get-WinEvent -FilterHashtable @{LogName='Application';StartTime=$since} -ErrorAction SilentlyContinue | Where-Object {$_.Message -match 'orbitdbwiz|Access is denied|blocked|prevented|restriction'} | Select-Object TimeCreated,ProviderName,Id,LevelDisplayName,Message | Format-List;Write-Host '===== FINISHED ====='"
===== 1 APPLOCKER POLICY =====


FilePath       : C:\Users\QXZ4Y4A\orbitdbwiz.exe
PolicyDecision : AllowedByDefault
MatchingRule   :



===== 2 COPY TO TEMP =====
C:\Users\QXZ4Y4A\AppData\Local\Temp\orbitdbwiz-test.exe NT AUTHORITY\SYSTEM:(F)
                                                        BUILTIN\Administrators:(F)
                                                        CHINA\QXZ4Y4A:(F)

Successfully processed 1 files; Failed processing 0 files
===== 3 RUN TEMP COPY =====
Program 'orbitdbwiz-test.exe' failed to run: Access is deniedAt line:1 char:503
+ ... $test;Write-Host '===== 3 RUN TEMP COPY =====';& $test --help;Write-H ...
+                                                    ~~~~~~~~~~~~~~.
At line:1 char:503
+ ... $test;Write-Host '===== 3 RUN TEMP COPY =====';& $test --help;Write-H ...
+                                                    ~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceUnavailable: (:) [], ApplicationFailedException
    + FullyQualifiedErrorId : NativeCommandFailed

Exit code: 0
===== 4 APPLOCKER EVENTS =====
===== 5 CODE INTEGRITY EVENTS =====


TimeCreated      : 2026/7/21 13:41:51
Id               : 3076
LevelDisplayName : Information
Message          : Code Integrity determined that a process (\Device\HarddiskVolume4\Windows\System32\cmd.exe) attempte
                   d to load \Device\HarddiskVolume4\Users\QXZ4Y4A\orbitdbwiz.exe that did not meet the Enterprise sign
                   ing level requirements or violated code integrity policy (Policy ID:{a244370e-44c9-4c06-b551-f6016e5
                   63076}). However, due to code integrity auditing policy, the image was allowed to load.

TimeCreated      : 2026/7/21 13:29:26
Id               : 3076
LevelDisplayName : Information
Message          : Code Integrity determined that a process (\Device\HarddiskVolume4\Windows\System32\cmd.exe) attempte
                   d to load \Device\HarddiskVolume4\Users\QXZ4Y4A\orbitdbwiz.exe that did not meet the Enterprise sign
                   ing level requirements or violated code integrity policy (Policy ID:{a244370e-44c9-4c06-b551-f6016e5
                   63076}). However, due to code integrity auditing policy, the image was allowed to load.



===== 6 DEFENDER EVENTS =====


TimeCreated      : 2026/7/21 14:35:06
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\AppData\Local\Temp\orbitdbwiz-test.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:27:18
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:26:38
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:14:59
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:14:18
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:14:18
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:13:41
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 14:13:22
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:46:20
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:45:35
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:44:24
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: containerfile:_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip; fil
                   e:_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip->orbitdbwiz.exe; webfile
                   :_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip|https://media.atc-github.
                   azure.cloud.bmw/releases/301045/files/74635?token=AAAPCM46K6534TJVKDVBJNLKL4KQK|pid:20560,ProcessSta
                   rt:134290862132149552
                        Detection Origin: Internet
                        Detection Type: FastPath
                        Detection Source: Downloads and attachments
                        User: NT AUTHORITY\SYSTEM
                        Process Name: Unknown
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:43:38
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: containerfile:_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip; fil
                   e:_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip->orbitdbwiz.exe; webfile
                   :_C:\Users\QXZ4Y4A\Downloads\orbitdbwiz_v0.1.47_x86_64-pc-windows-msvc.zip|https://media.atc-github.
                   azure.cloud.bmw/releases/301045/files/74635?token=AAAPCM46K6534TJVKDVBJNLKL4KQK|pid:20560,ProcessSta
                   rt:134290862132149552
                        Detection Origin: Internet
                        Detection Type: FastPath
                        Detection Source: Downloads and attachments
                        User: CHINA\QXZ4Y4A
                        Process Name: Unknown
                        Security intelligence Version: AV: 1.455.241.0, AS: 1.455.241.0, NIS: 1.455.241.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:25:07
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 13:24:23
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 12:59:09
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 12:58:27
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 12:18:21
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_c:\users\qxz4y4a\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: System
                        User: NT AUTHORITY\SYSTEM
                        Process Name: Unknown
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 12:17:28
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_c:\users\qxz4y4a\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: System
                        User: NT AUTHORITY\SYSTEM
                        Process Name: Unknown
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:34:26
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:33:42
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:28:05
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:27:21
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:20:59
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:20:15
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:19:37
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:18:49
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:10:17
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:09:33
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 11:00:00
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:59:11
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:49:56
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:49:10
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:45:27
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:44:39
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:40:51
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:40:01
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:39:18
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:38:25
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:09:35
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:08:47
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:05:36
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 10:04:40
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.235.0, AS: 1.455.235.0, NIS: 1.455.235.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:48:32
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:47:44
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:47:44
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:47:01
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:46:53
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:45:16
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:44:22
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:44:13
Id               : 1117
LevelDisplayName : Information
Message          : Microsoft Defender Antivirus has detected malware or potentially unwanted software using Defender se
                   curity intelligence and applied the remediation action defined in the security settings.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: NT AUTHORITY\SYSTEM
                        Process Name: C:\Windows\System32\cmd.exe
                        Action: Block
                        Action Status:  No additional actions required
                        Error Code: 0x00000000
                        Error description: The operation completed successfully.
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008

TimeCreated      : 2026/7/21 9:43:24
Id               : 1116
LevelDisplayName : Warning
Message          : Microsoft Defender Antivirus has detected malware or other potentially unwanted software.
                    For more information please see the following:
                   https://go.microsoft.com/fwlink/?linkid=37020&name=EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl&thr
                   eatid=2147757445&enterprise=1
                        Name: EUS:Win32/CustomEnterpriseNoAlertBlockOnly!cl
                        ID: 2147757445
                        Severity: Severe
                        Category: Enterprise Unwanted Software
                        Path: file:_C:\Users\QXZ4Y4A\tspaws-cli\orbitdbwiz.exe
                        Detection Origin: Local machine
                        Detection Type: FastPath
                        Detection Source: Real-Time Protection
                        User: CHINA\QXZ4Y4A
                        Process Name: C:\Windows\System32\cmd.exe
                        Security intelligence Version: AV: 1.455.226.0, AS: 1.455.226.0, NIS: 1.455.226.0
                        Engine Version: AM: 1.1.26060.3008, NIS: 1.1.26060.3008



===== 7 SOFTWARE RESTRICTION POLICY =====

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers
    authenticodeenabled    REG_DWORD    0x0

ERROR: The system was unable to find the specified registry key or value.
===== 8 APPLICATION EVENTS =====
===== FINISHED =====

C:\Users\QXZ4Y4A>
