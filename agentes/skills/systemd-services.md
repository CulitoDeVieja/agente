# systemd-services

## Propósito
Gestionar servicios del sistema con systemd en VPS Linux.

## Cuándo usar
Auditor + Ops, para iniciar, detener, reiniciar o diagnosticar servicios systemd.

## Pasos
1. Ver estado: `systemctl status <servicio>`
2. Iniciar: `systemctl start <servicio>`
3. Detener: `systemctl stop <servicio>`
4. Reiniciar: `systemctl restart <servicio>`
5. Habilitar al boot: `systemctl enable <servicio>`
6. Ver logs: `journalctl -u <servicio> -n 50 --no-pager`

## Ejemplo
```bash
systemctl status ciclo-agente
journalctl -u ciclo-agente -n 50 --no-pager
systemctl restart ciclo-agente
```

## Errores comunes
- `Unit not found` → el archivo `.service` no existe en `/etc/systemd/system/`; crearlo o verificar nombre.
- Cambios al `.service` no toman efecto → correr `systemctl daemon-reload` antes de restart.
- Servicio cae inmediatamente → revisar `journalctl -u <servicio> -xe` para error de arranque.
- Sin permisos → anteponer `sudo` si no se es root.
