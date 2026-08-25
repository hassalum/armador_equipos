# Armador de equipos

App web para armar los equipos de un partido: cargás los datos de la cancha, los jugadores con su puesto, repartís los equipos arrastrando y te llevás la imagen lista para mandar al grupo.

## Flujo

1. **Partido y jugadores** — dirección, establecimiento, fecha y hora, valor y comentario (todo opcional salvo los nombres). Debajo, los jugadores: escribís un nombre y se abre solo el casillero siguiente. Al lado de cada uno, cuatro puestos opcionales:
   - guantes → arquero (máximo 2 en total, uno por equipo)
   - escudo → defensor
   - brújula → medio
   - espada → delantero
2. **Equipos** — pop up con todos los nombres. Los arrastrás a Claro u Oscuro (o usás los botones `Osc` / `Cla`, que andan en celular). Arriba de cada tablero elegís la formación.
3. **Suplentes** — pregunta sí/no. Si sí, cargás suplentes por equipo, también con puesto opcional.
4. **Imagen** — la cancha renderizada. Se exporta a PNG (2x/3x), se copia al portapapeles o se baja como SVG.

## Reglas de armado

- **Los puestos se asignan según el icono**: cada jugador cae en la línea que le corresponde. Los que no tienen puesto marcado se sortean entre los lugares que quedan.
- **Se puede reacomodar**: en la pantalla final, tocás un jugador y después otro (o un puesto libre) y se intercambian. Funciona también entre equipos.
- **Si faltan jugadores** para la formación elegida, el puesto queda dibujado punteado con la leyenda "Libre".
- **Si sobran**, la app avisa y no deja guardar hasta que agrandes la formación o saques a alguien.
- Los suplentes aparecen al costado de su equipo en la imagen final.

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
- `dibujar()` — genera el SVG completo (encabezado, cancha, jugadores, suplentes, pie).
- `:root` en el CSS — la paleta de la interfaz.

El estado completo se guarda en `localStorage` bajo la clave `armador:v2`, así podés cerrar y retomar. El botón **Nuevo partido** lo limpia.

## Licencia

MIT.
