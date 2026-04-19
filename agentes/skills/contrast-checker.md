# contrast-checker

## Propósito
Verificar que las combinaciones de color de texto/fondo cumplan los requisitos de contraste WCAG.

## Cuándo usar
- Al definir la paleta de colores del producto
- Al elegir color de texto sobre un fondo de color
- Antes de aprobar un diseño para producción

## Pasos

### Estándares WCAG

| Nivel | Texto normal | Texto grande (18px+ o 14px bold) |
|-------|-------------|----------------------------------|
| AA    | 4.5:1       | 3:1                              |
| AAA   | 7:1         | 4.5:1                            |

### Herramientas

1. **Browser DevTools**: Chrome → Elements → color swatch → "Contrast ratio"

2. **CLI con Storybook a11y**:
   ```bash
   npm install -D @storybook/addon-a11y
   ```
   Agrega el addon en `.storybook/main.ts` y cada story muestra el contraste en el panel a11y.

3. **Script de verificación (Node.js)**:
   ```ts
   // Fórmula WCAG de luminancia relativa
   function relativeLuminance(hex: string): number {
     const rgb = hex.match(/\w\w/g)!.map(c => {
       const v = parseInt(c, 16) / 255;
       return v <= 0.03928 ? v / 12.92 : ((v + 0.055) / 1.055) ** 2.4;
     });
     return 0.2126 * rgb[0] + 0.7152 * rgb[1] + 0.0722 * rgb[2];
   }

   function contrastRatio(fg: string, bg: string): number {
     const l1 = relativeLuminance(fg);
     const l2 = relativeLuminance(bg);
     const [light, dark] = l1 > l2 ? [l1, l2] : [l2, l1];
     return (light + 0.05) / (dark + 0.05);
   }

   const ratio = contrastRatio('#cdd6f4', '#1e1e2e');
   console.log(`Contraste: ${ratio.toFixed(2)}:1`); // → 10.5:1 ✓
   ```

4. **Recursos online**:
   - https://webaim.org/resources/contrastchecker/
   - https://coolors.co/contrast-checker
   - Plugin Figma: "Contrast" de Stark

### Colores seguros para texto sobre fondos oscuros (dark theme):
- Fondo `#1e1e2e` → texto `#cdd6f4` → 10.5:1 (AAA)
- Fondo `#313244` → texto `#cdd6f4` → 6.5:1 (AA)

## Ejemplo
Texto muted `#6c7086` sobre `#1e1e2e` da 3.2:1 — pasa AA para texto grande pero no para texto pequeño.

## Errores comunes
- **Verificar solo el estado default**: revisar también hover, focus, placeholder (placeholders suelen fallar).
- **Asumir que "gris claro en blanco" pasa**: muchos grises populares (#999 sobre #fff) fallan el AA.
- **No verificar dark mode**: si la app tiene dark mode, verificar los dos temas por separado.
