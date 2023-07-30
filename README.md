# Amsi Bypass on Windows 11 In 2023
Technical details can be found in the article: <https://medium.com/@gustavshen/bypass-amsi-on-windows-11-75d231b2cac6>

## Attack_AmsiOpenSession.ps1

```c++
HRESULT AmsiOpenSession(
[in] HAMSICONTEXT amsiContext,
[out] HAMSISESSION *amsiSession
);
```

This powershell script can be used to bypass AMSI by patching AmsiOpenSession. According to the assemble codes, if **any** of the following conditions are met, the function will exit with `E_INVALIDARG` error. 
1. RCX is 0
2. RDX is 0
3. The 2nd QWORD of HAMSICONTEXT structure is 0
4. The 3rd QWORD of HAMSICONTEXT structure is 0


![image](/screenshot/amsiopensession.jpg)

This script patches AmsiOpenSession by setting RCX to 0.

![image](/screenshot/opensession_bypass.jpg)

**Patching AmsiOpenSession cannot bypass AMSI for Assembly.Load()**

## Attack_AmsiScanBuffer.ps1

```c++
HRESULT AmsiScanBuffer(
[in] HAMSICONTEXT amsiContext,
[in] PVOID buffer,
[in] ULONG length,
[in] LPCWSTR contentName,
[in, optional] HAMSISESSION amsiSession,
[out] AMSI_RESULT *result
);
```
This powershell script can be used to bypass AMSI by patching AmsiScanBuffer. The script patches AmsiScanBuffer by setting RAX to the value of error `E_INVALIDARG` and return immediately.

![image](/screenshot/scanbuffer_bypass.jpg)

**Patching AmsiScanBuffer can bypass AMSI for Assembly.Load()**

![image](/screenshot/scanbuffer_dnet.jpg)

## one-liner-payload.md

This file contains one-liner payloads that can be used in the current powershell session and immediately bypass AMSI. However, it cannot bypass AMSI for `Assembly.Load()`.

Details can be found in this file.
