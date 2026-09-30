# SaborUPC · design-system

**Equipo:** Plataforma · **Dueño:** Deimis · **Puerto local:** 8085

Sistema de diseño mínimo: `tokens.css` con variables CSS (colores, tipografía, espaciado y radio).
**Solo UI, nada de lógica de negocio.** Cada equipo consume este archivo desde su propio micro frontend,
así los tres se ven iguales sin tener que ponerse de acuerdo en cada color.

## Cómo ejecutar

```bash
python -m http.server 8085
```

- http://localhost:8085 → muestra visual de los tokens (con el valor que el navegador está usando)
- http://localhost:8085/tokens.css → la hoja que consumen el contenedor y los 4 micro frontends

## Los tokens

| Variable | Valor | Uso típico |
|---|---|---|
| `--sabor-color-primario` | `#0b4f8a` | cabecera, botones, enlaces activos |
| `--sabor-color-secundario` | `#1b7a3e` | confirmaciones, carrito, botón hover |
| `--sabor-color-acento` | `#f2c200` | insignias y contadores |
| `--sabor-color-fondo` | `#f4f6f8` | fondo de página |
| `--sabor-color-texto` | `#222222` | texto principal |
| `--sabor-color-error` | `#a12a1e` | mensajes de error |
| `--sabor-fuente` | `Arial, Helvetica, sans-serif` | tipografía de toda la app |
| `--sabor-espacio-s` | `8px` | relleno pequeño |
| `--sabor-espacio-m` | `16px` | relleno y separaciones normales |
| `--sabor-espacio-l` | `24px` | separación de bloques, `gap` del header |
| `--sabor-radio` | `4px` | esquinas de botones y tarjetas |

## Cómo se consume

```html
<link rel="stylesheet" href="http://localhost:8085/tokens.css">
```

```css
/* siempre con respaldo: si el 8085 se cae, la app se sigue viendo bien */
.boton {
  background: var(--sabor-color-primario, #0b4f8a);
  color: #fff;
  padding: var(--sabor-espacio-s, 8px) var(--sabor-espacio-m, 16px);
  border-radius: var(--sabor-radio, 4px);
}
```

El respaldo no es decorativo: permite trabajar en un micro frontend sin levantar el 8085, y evita que
un origen caído se convierta en una pantalla en blanco.

## Reglas para los equipos

- **Solo UI.** Aquí no van colores de negocio ni textos.
- **Nada nuevo sin avisar.** Si un MFE necesita un token que no existe, se avisa al grupo y se agrega
  a este archivo; nadie se lo inventa por su cuenta, o los tres equipos terminan con paletas distintas.
- **El valor del respaldo debe coincidir con el del token.** Así, con el 8085 arriba o abajo, la app se ve igual.
- Los micro frontends con Shadow DOM (los web components) sí heredan estas variables: el Shadow DOM
  aísla estilos, pero las propiedades CSS personalizadas atraviesan el shadow boundary.

## Cómo probarlo

1. `python -m http.server 8085` y abrir http://localhost:8085: cada token aparece con su valor real.
2. Cambiar un valor en `tokens.css`, recargar la página de muestra: el cambio se ve al instante.
3. Con el contenedor arriba (puerto 8080), cambiar `--sabor-color-primario` y recargar http://localhost:8080:
   la cabecera toma el color nuevo, lo que demuestra que el token viaja por su propio origen.
4. Apagar el 8085: la app sigue funcionando con los respaldos, sin cambios visibles.
