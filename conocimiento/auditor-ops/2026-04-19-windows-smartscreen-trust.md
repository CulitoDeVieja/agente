# Windows SmartScreen — reputación para publishers nuevos

## TL;DR
- SmartScreen calcula reputación por hash + por certificado del publisher; arranca en 0.
- Desde 03/2024 los certs **EV ya NO eliminan el warning instantáneo**: también deben ganarse reputación.
- No hay portal de "whitelist" para devs; reputación = volumen de descargas + tiempo + sin reportes.

## Detalles
Recetas para reducir warnings en releases de panel-agentes (Tauri Windows):
1. Firmar TODOS los EXE/MSI con el MISMO cert (OV o EV) consistentemente.
2. NO regenerar el cert por release (resetea reputación).
3. Subir el binario a Microsoft Defender submission portal en cada release nuevo.
4. Aceptar que las primeras semanas/meses los usuarios verán "Más información → Ejecutar de todos modos".

Para owners pequeños sin presupuesto de cert (>$300/año): documentar en README los pasos para que el usuario apruebe el binario manualmente (ver auditor-ops-89 SmartScreen).

## Fuente
- https://learn.microsoft.com/en-us/answers/questions/5857071/how-can-a-small-software-publisher-build-smartscre
- https://blog.betterbird.eu/2026/01/whats-the-story-with-windows-smartscreen
- https://www.digicert.com/blog/ms-smartscreen-application-reputation

## Aplicabilidad
**Sí — crítico**. panel-agentes target Windows. Sin certificado al principio: el warning va a aparecer. Documentar workflow del usuario y planificar cert OV una vez haya tracción real.
