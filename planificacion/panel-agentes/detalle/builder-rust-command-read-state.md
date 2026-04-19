# Plan: rust-command-read-state

## Objetivo
Implementar comando Tauri `read_state()` que parsea `STATE.md`.

## Signature
```rust
#[tauri::command]
fn read_state() -> StateSnapshot
```

## Pasos
1. Leer `STATE.md` del repo local
2. Parsear tabla markdown de agentes (regex por fila)
3. Extraer rol, ubicación, última señal
4. Detectar si hay `MODO MASTER activo` en el archivo
5. Retornar `StateSnapshot`

## Tests planeados
- [ ] Parsea correctamente las 4 filas de agentes
- [ ] Detecta `✅ activo` vs `⏳ esperando`
- [ ] `modoMaster: true` cuando badge presente
