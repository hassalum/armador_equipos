# Armador de equipos

App web para armar los equipos de un partido: cargás los datos de la cancha, los jugadores con su puesto, repartís los equipos arrastrando y te llevás la imagen lista para mandar al grupo.

## Flujo

1. **Partido y jugadores** — tamaño de cancha (fútbol 5, 6, 7, 8, 9 u 11), dirección, establecimiento, fecha y hora, valor y comentario. El tamaño define cuántos puestos tiene cada equipo y qué formaciones se ofrecen. Debajo, los jugadores: escribís un nombre y se abre solo el casillero siguiente. Al lado de cada uno, cuatro puestos opcionales:
   - guantes → arquero (máximo 2 en total, uno por equipo)
   - escudo → defensor
   - brújula → medio
   - espada → delantero
2. **Equipos** — pop up con todos los nombres. Los arrastrás a Claro u Oscuro (o usás los botones `Osc` / `Cla`, que andan en celular). Arriba de cada tablero elegís la formación.
3. **Suplentes** — pregunta sí/no. Si sí, cargás suplentes por equipo, también con puesto opcional.
4. **Imagen** — la cancha renderizada. Se exporta a PNG (2x/3x), se copia al portapapeles o se baja como SVG. Abajo queda el bloque de **Compartir**: el resumen en texto (listo para pegar en WhatsApp) y dos atajos, uno a Google Maps con la dirección y otro al buscador de Google con el nombre del establecimiento. El botón Compartir usa la Web Share API del celular y manda la imagen junto con el texto.

## Reglas de armado

- **El tamaño de cancha manda**: la formación de cada equipo tiene que sumar exactamente esa cantidad. Si no coincide, la app avisa y no deja guardar.
- **Los puestos se asignan según el icono**: cada jugador cae en la línea que le corresponde. Los que no tienen puesto marcado se sortean entre los lugares que quedan.
- **Se puede reacomodar**: en la pantalla final, tocás un jugador y después otro (o un puesto libre) y se intercambian. Funciona también entre equipos.
- **Si faltan jugadores** para la formación elegida, el puesto queda dibujado punteado con la leyenda "Libre".
- **Si sobran**, la app avisa y no deja guardar hasta que agrandes la formación o saques a alguien.
- Los suplentes aparecen al costado de su equipo en la imagen final.
- Al elegir un puesto, debajo de la fila aparece el nombre en verde ("Arquero", "Defensor", "Medio", "Delantero"), porque en celular no se ve el tooltip del icono.

## Correrlo local

Sin dependencias ni build. Abrís `index.html`, o levantás un server:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

## Publicarlo en GitHub Pages

```bash
git add index.html README.md
git commit -m "Armador de equipos"
git push
```

En el repo: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `(root)`**.
Ojo: si el repo es privado, Pages solo funciona con cuenta paga; si no, pasalo a público.

## Estructura

Todo vive en `index.html`. Puntos de entrada si querés tocar algo:

- `ICONOS` — los cuatro iconos de puesto, en paths de 24x24. Se usan igual en la UI y adentro de la cancha.
- `slots(formacion)` — traduce `"1-3-2"` a la lista de puestos con su rol y su coordenada normalizada (`a` = avance desde el arco propio, `c` = ancho). La orientación horizontal/vertical es solo un mapeo distinto de esas coordenadas.
- `autoAsignar(eq)` — el reparto por rol y el sorteo de los que quedan.
- `dibujar()` — genera el SVG completo (encabezado, cancha, jugadores, suplentes, pie). La marca de agua "HS" del césped se hace con un `clipPath` sobre el texto y franjas alternadas adentro, para que parezca cortada con la máquina.
- `PELOTA`, `pelotaCruza()`, `lluviaPelotas()` — las animaciones. Respetan `prefers-reduced-motion`.
- `textoResumen()`, `urlMapa()`, `urlBusqueda()` — el texto para compartir y los atajos a Google.
- `:root` en el CSS — la paleta de la interfaz.

El estado completo se guarda en `localStorage` bajo la clave `armador:v2`, así podés cerrar y retomar. El botón **Nuevo partido** lo limpia.

## Licencia

MIT.
