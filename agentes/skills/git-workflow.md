# git-workflow

## Propósito
Evitar conflictos y commits rotos.

## Cuándo usar
Siempre que un agente vaya a escribir al repo.

## Pasos
1. `git pull --rebase` **antes** de cualquier cambio.
2. Trabajar en worktree propio si hay riesgo de choque.
3. Commit con prefijo del rol (ej: `feat:`, `ORCH:`, `skills:`).
4. `git pull --rebase` **antes** de push (por si entró algo durante tu trabajo).
5. `git push`. Si falla → rebase otra vez → push.

## Ejemplo
```
git pull --rebase
# ...editás archivos...
git add <archivo>
git commit -m "feat: paso 45 implementado"
git pull --rebase
git push
```

## Errores comunes
- `non-fast-forward` → no hiciste pull antes. Solución: `git pull --rebase && git push`.
- Conflicto de merge durante rebase → resolver el archivo manualmente, `git add`, `git rebase --continue`.
- Push rechazado 3 veces seguidas → escalar al orchestrator (puede haber bucle).

## Prohibido
- `git push --force` a main sin aprobación del owner.
- `git reset --hard` sobre commits de otros agentes.
- Amend a commits ya pusheados.
