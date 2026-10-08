# preflight

Preflight checks a file for malware "red flags" (suspicious behaviour), never
executing it; it only reads the file.

**Download:** https://github.com/cooldood155/preflight-releases/releases/latest

## Setup (one time only, ~1 minute)

1. On the release page, under **Assets**, click **preflight.exe**.
   If your browser warns you, choose **Keep** (or **Keep anyway**).
   - Your browser should have no issues with this, I have thoroughly ensured
     the program contains nothing deamed *suspicious* that malware detectors
     may flag or block.
2. Open your Downloads folder, right-click **preflight.exe**, click
   **Properties**.
3. At the bottom of the window, tick the **Unblock** box, then click **OK**.
4. Drag preflight.exe onto your Desktop, or store it wherever you want.
   - If it's not on your desktop directly, you can right-click the
     preflight.exe (in its final stored location) and click **Show more
     options** then **Create Shortcut** (that shortcut can be moved to anywhere
     (like your desktop)).

Want to be sure the preflight.exe is legit? Compare it with the SHA-256
on the release page via PowerShell.

**In PowerShell**, inside your Downloads folder, or wherever the preflight.exe
is located (easily done by opening file explorer, going to the folder with
preflight.exe, hold **Shift** while right-clicking and choose **Open PowerShell
window here**), run this command to check:

```PowerShell
$h = [regex]::Match((Read-Host '31e40b2b06079816fec4e703bdc569d5c7c1b99be5f78efd2e22a9d439a18ef1'), '[0-9a-fA-F]{64}').Value; if ($h -and (Get-FileHash .\preflight.exe).Hash -eq $h) { 'MATCH - legit executable' } else { 'NO MATCH - malformed/malicious executable' }
```

When it asks, paste the SHA-256 code from the release page and press Enter
(where you downloaded the file).

## Checking a file

Drag the file, or the whole folder it's in (optionally containing multiple
executables), onto preflight.exe. Read the *VERDICT* at the bottom, then press
Enter to close.

- **DANGEROUS**: *possibly* delete it, *possibly* never run it (should never
  false flag a program as dangerous from my testing).
- **SUSPICIOUS**: if it is not from a trusted source, ask *me* (@cooldood155)
  if I think it is fine if you feel like you need to.
- **NO RED FLAGS FOUND**: a good sign, but NOT a guarantee (I can't test and look at everything).

## Updating

Each new version is a new file. Download it from the newest release and do
the Unblock step again.

Only trust preflight from `github.com/cooldood155/preflight-releases`. If
anyone sends it to you another way, do not run it.
