# Windows Defender — exclusiones para usuarios

## TL;DR
- Usuario puede excluir tu .exe vía: Windows Security UI / PowerShell / GPO / Intune.
- PowerShell admin: `Add-MpPreference -ExclusionProcess "C:\Path\panel-agentes.exe"`.
- ⚠️ Exclusiones reducen seguridad — documentar en README solo como workaround si hay false positive confirmado.

## Detalles

**Para README troubleshooting de panel-agentes:**

Si Defender bloquea/cuarentena el .exe (false positive típico de apps Tauri sin firma):

**Opción 1 — UI:**
1. Configuración → Privacidad y seguridad → Seguridad de Windows.
2. Protección antivirus y amenazas → Administrar configuración.
3. Exclusiones → Agregar exclusión → Archivo → seleccionar `panel-agentes.exe`.

**Opción 2 — PowerShell admin:**
```powershell
Add-MpPreference -ExclusionPath "C:\Users\$env:USERNAME\AppData\Local\panel-agentes"
Add-MpPreference -ExclusionProcess "panel-agentes.exe"
```

**Opción 3 — Reportar a Microsoft (mejor camino):**
https://www.microsoft.com/wdsi/filesubmission → submit como "False positive". Defender baja el flag globalmente en horas/días.

⚠️ Variables como `%USERPROFILE%` NO se interpretan en exclusiones — usar path completo.

## Fuente
- https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus
- https://learn.microsoft.com/en-us/defender-endpoint/configure-process-opened-file-exclusions-microsoft-defender-antivirus
- https://www.elevenforum.com/t/add-or-remove-exclusions-for-microsoft-defender-antivirus-in-windows-11.8797/

## Aplicabilidad
**Sí — documentación usuario.** Reemplaza/complementa auditor-ops-088 (antivirus false positives). Default: submit a Microsoft. Workaround temporal: instructions en README.
