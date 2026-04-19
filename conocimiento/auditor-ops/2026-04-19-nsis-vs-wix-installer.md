# NSIS vs WiX — installer Windows para Tauri

## TL;DR
- **NSIS** (.exe setup) — fast, multi-language built-in, **default Tauri 2.x**.
- **WiX** (.msi) — enterprise, system integration, requiere Windows host para build.
- Tauri permite emitir ambos: `bundle.targets: ["nsis", "msi"]`.

## Detalles

| Criterio | NSIS | WiX (MSI) |
|---|---|---|
| Output | `-setup.exe` | `.msi` |
| Cross-compile | ✅ Linux/macOS pueden buildear | ❌ Solo Windows |
| Multi-idioma | Built-in (un installer todos los idiomas) | Por release |
| Enterprise (GPO, Intune) | Limitado | ✅ Nativo |
| Update silencioso | Custom scripts | Built-in (msiexec /quiet) |
| Tamaño overhead | Mínimo (~80kb runtime) | Mayor |
| Curva aprendizaje | Baja (IDE Wizard) | Alta (XML verbose) |

**Config Tauri (`src-tauri/tauri.conf.json`):**
```json
"bundle": {
  "targets": ["nsis"],   // o ["nsis", "msi"] para ambos
  "windows": {
    "nsis": {
      "languages": ["English", "Spanish", "PortugueseBR"],
      "displayLanguageSelector": true
    }
  }
}
```

**Recomendación panel-agentes:** NSIS solo. WiX agregar solo si owner pide deploy enterprise (Active Directory/GPO).

## Fuente
- https://v2.tauri.app/distribute/windows-installer/
- https://github.com/tauri-apps/tauri/issues/6859
- https://blog.doubleslash.de/en/software-technologien/nsis-vs-wix-a-comparison-of-two-installers/

## Aplicabilidad
**Sí — decisión de bundling.** Para v0.1.0 panel-agentes: NSIS. Reemplaza auditor-ops-092 (deploy MSI scope per-user).
