# Alternativas a signtool.exe para code signing Windows

## TL;DR
- `signtool.exe` (Windows SDK) sigue siendo el default; todo lo demás lo wrappea.
- **AzureSignTool** (vcsjones) — usa Azure Key Vault para no exponer la clave privada.
- **jsign** (Java, cross-platform) — firma Authenticode desde Linux/macOS CI.

## Detalles
| Tool | Cuándo usar |
|---|---|
| `signtool.exe` | Build local Windows con cert en disco/HSM USB |
| AzureSignTool | CI con cert en Azure Key Vault — no clave en repo, rota fácil |
| jsign | CI runner Linux que necesita firmar binarios Windows |
| EZSignIt | GUI/CLI gratis, redistribuible, sin Windows SDK |
| DigiCert SMCTL | Si el cert es de DigiCert KeyLocker |

Ejemplo AzureSignTool:
```bash
azuresigntool sign \
  -kvu https://my-vault.vault.azure.net \
  -kvc cert-name \
  -kvm \
  -tr http://timestamp.digicert.com \
  -td sha256 \
  panel-agentes.exe
```

## Fuente
- https://github.com/vcsjones/AzureSignTool
- https://learn.microsoft.com/en-us/windows/win32/seccrypto/signtool
- https://weblog.west-wind.com/posts/2025/Jul/20/Fighting-through-Setting-up-Microsoft-Trusted-Signing

## Aplicabilidad
**Parcial.** Para v0.1.0 panel-agentes sin presupuesto cert: usar `tauri-action` self-signed (warning, ok). Cuando haya cert real → AzureSignTool en CI (no exponer .pfx en GitHub Secrets).
