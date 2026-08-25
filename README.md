# Armador de equipos

App web para armar los equipos de un partido: cargás los datos de la cancha y los jugadores, repartís los equipos arrastrando y te llevás la imagen lista para mandar al grupo.

## Flujo

Tres pasos.

1. **El partido y los jugadores** — tamaño de cancha (fútbol 5, 6, 7, 8, 9 u 11), establecimiento, dirección, fecha y hora, valor y comentario. Lo único obligatorio son los nombres. Debajo, la lista: escribís un nombre y se abre el casillero siguiente. Cada jugador tiene cinco marcas opcionales:
   - **Arquero** (guantes), **Defensa** (escudo), **Medio** (brújula), **Delantero** (espada) — definen en qué línea cae
   - **Suplente** (banco) — lo saca de la cancha y lo manda al banco de su equipo
2. **Los equipos** — arrastrás cada nombre a Claro u Oscuro, o usás los botones `Osc` / `Cla` (que andan en celular, donde el drag nativo no funciona). Cada tablero muestra los que van en cancha y, aparte, su banco. Ahí también elegís la formación y el color.
3. **La imagen** — la cancha renderizada, con export a PNG (2x/3x), copiar al portapapeles o bajar el SVG. Abajo queda el bloque para pasarlo al grupo: el resumen en texto listo para WhatsApp y dos atajos, uno a Google Maps con la dirección y otro al buscador de Google con el nombre del establecimiento. El botón Compartir usa el menú nativo del celular y manda la imagen junto con el texto.

## Reglas de armado

- **El tamaño de cancha manda**: la formación de cada equipo tiene que sumar exactamente esa cantidad. Si no coincide, avisa y no deja seguir.
- **Los puestos se asignan según el icono**: cada jugador cae en la línea que le corresponde. Los que no tienen puesto marcado se sortean entre los lugares que quedan.
- **Los suplentes no ocupan puesto**: no entran en el sorteo de la formación y salen listados en el banco, al costado de su equipo en la imagen.
- **Se puede reacomodar**: en la pantalla final, tocás un jugador y después otro (o un puesto libre) y se intercambian. Funciona también entre equipos.
- **Si faltan jugadores**, el puesto queda dibujado punteado con la leyenda "Libre".
- **Si sobran en cancha**, avisa: los mandás al banco o agrandás la formación.
- **Arqueros**: hasta dos en cancha, uno por equipo. Los del banco no cuentan para ese tope.

## Correrlo local

Sin dependencias ni build. Abrís `index.html`, o levantás un server:

```
python3 -m http.server 8000
# http://localhost:8000
```

## Publicarlo en GitHub Pages

Subís los archivos a `main` y en el repo: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `(root)`**.

## Estructura

Todo vive en `index.html`. Puntos de entrada si querés tocar algo:

- `:root` en el CSS — la paleta. La dirección visual es "partido de noche bajo reflectores": fondo verde-negro, acento de luz cálida y un cono de luz difusa en el encabezado (`body::before`).
- `ICONOS` — los cinco iconos (cuatro puestos y el banco), en paths de 24x24. Se usan igual en la interfaz y adentro de la cancha.
- `slots(formacion)` — traduce `"1-3-2"` a la lista de puestos con su rol y su coordenada normalizada (`a` = avance desde el arco propio, `c` = ancho). La orientación horizontal/vertical es solo un mapeo distinto de esas coordenadas.
- `titularesDe(eq)` / `suplentesDe(eq)` — el corte entre cancha y banco.
- `autoAsignar(eq)` — el reparto por rol y el sorteo de los que quedan.
- `dibujar()` — genera el SVG completo. La marca de agua "HS" del césped se hace con un `clipPath` sobre el texto y franjas alternadas adentro, para que parezca cortada con la máquina.
- `textoResumen()`, `urlMapa()`, `urlBusqueda()` — el texto para compartir y los atajos a Google.
- `PELOTA`, `pelotaCruza()`, `lluviaPelotas()` — las animaciones. Respetan `prefers-reduced-motion`.

El estado se guarda en `localStorage` bajo la clave `armador:v3`. El botón **Empezar de nuevo** lo limpia.

## Licencia

MIT.
