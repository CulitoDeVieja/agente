# railway-deploy

## Propósito
Deployar un servicio a Railway desde CLI.

## Cuándo usar
Auditor + Ops, cuando hay que hacer deploy o redeploy de un servicio en Railway.

## Pasos
1. `railway login` — autenticar si no hay sesión activa.
2. `railway link` — vincular directorio al proyecto Railway correcto.
3. `railway up` — deployar el código actual.
4. `railway logs` — verificar que el servicio arrancó sin errores.
5. Si falla: `railway logs --tail 100` para ver el error completo.

## Ejemplo
```bash
cd /opt/mi-servicio
railway link
railway up
railway logs
```

## Errores comunes
- `Not linked to a project` → correr `railway link` primero.
- Deploy exitoso pero servicio cae → revisar variables de entorno con `railway variables`.
- Build falla por dependencias → verificar `package.json` o `requirements.txt` actualizados.
- Puerto incorrecto → Railway usa `PORT` env var; asegurarse de que la app lo lee.
