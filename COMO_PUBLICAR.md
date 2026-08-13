# Cómo se publica una revisión de cola

Tres ficheros y un comando. **No se escribe HTML a mano nunca.**

```
plantilla.html          el diseño, con marcadores. No se toca salvo rediseño
publicar.py             lee el xlsx, calcula los números y genera la página
contenido-AAAA-MM-DD.json   lo único que se escribe en cada revisión
```

## El comando

```bash
python3 publicar.py barrido.xlsx \
  --fecha 2026-08-14 \
  --version 0.30.0 \
  --contenido contenido-2026-08-14.json

git add -A
git commit -m "revision del 14 de agosto"
git push origin main
```

GitHub Pages construye solo en unos veinte segundos. La URL es
`https://nicostocchero.github.io/moreno-revision/`.

## Qué hace el script

1. Lee la hoja de leads del xlsx. Si el nombre de la hoja contiene "lead", la coge.
2. Calcula solo los conteos: hilos por grupo, borradores, clientes, reparto de confianza.
3. Rellena `plantilla.html`.
4. Escribe `AAAA-MM-DD/index.html` y copia esa misma página a la raíz.
5. Actualiza el bloque de revisiones anteriores con todas las carpetas que encuentre.

Si queda un marcador sin rellenar, **para y avisa** en vez de publicar una página rota.

## El fichero de contenido

Es lo único que hay que escribir. Estructura:

```json
{
  "sub": "opcional, si no se pone se genera con los conteos reales",
  "controles": [
    {"lab": "Control · ficha ausente", "val": "0 de 3", "note": "por qué importa"}
  ],
  "cambios": [
    {"titulo": "...", "estado": "Hecho | Pendiente | Fuera de MORENO",
     "antes": "...", "ahora": "...", "cita": "opcional, entre comillas"}
  ],
  "sugerencias": [
    {"titulo": "...", "quien": "Decisión técnica · Nico",
     "texto": ["primer párrafo", "segundo párrafo"]}
  ]
}
```

Se puede copiar el del día anterior y cambiar lo que haya cambiado. Los `controles`
son los cuatro números del encabezado, así que se ponen los del barrido nuevo con su
comparación contra el anterior.

## Estructura de la web

```
/                    la revisión más reciente
/AAAA-MM-DD/         cada revisión, archivada por fecha
/legacy/             la primera, del 4 de agosto
/sistema.html        el explicador del sistema
```

## Aviso

La web es pública y lleva nombres de contactos y empresas reales. El `noindex` y el
`robots.txt` solo hablan con los buscadores: cualquiera con la URL entra. Está
pendiente moverla a un sitio con control de acceso.
