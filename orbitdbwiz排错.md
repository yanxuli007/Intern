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
