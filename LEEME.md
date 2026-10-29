# Geoportal Costa Rica

Geoportal web de datos abiertos de Costa Rica, con la **Municipalidad de Corredores** como entidad
publicadora de ejemplo. Corre sin servidor (sitio estático) sobre la interfaz libre Isocarta.

**Sitio publicado:** https://agricien.github.io/geoportal-costa-rica/

> Demostración. No es un sitio oficial del Gobierno de Costa Rica ni de la Municipalidad de Corredores.
> Los datos vienen de servicios públicos y están simplificados para verlos en el navegador.

## Qué trae

| Grupo | Capas | Origen |
|---|---|---|
| Límites y población | provincias, cantones, distritos con población estimada, territorios indígenas | IGN, INEC, MEP |
| Hidrografía | ríos principales, cuencas | HydroSHEDS, CENIGA |
| Transporte y servicios | red vial nacional, aeropuertos, EBAIS, establecimientos de salud, centros educativos, líneas de transmisión | MOPT, OurAirports, INEC/CCSS, Healthsites, MEP, ICE |
| Ambiente | áreas silvestres protegidas, corredores biológicos, áreas de conservación, humedales, zonas de vida | SINAC, CENIGA |
| Amenazas | fallas del Cuaternario, áreas de inundación, sismos M ≥ 5 | CNE, USGS |
| Municipalidad de Corredores | límite distrital, barrios, mapa catastral (18 708 predios), zonas homogéneas, zonas agropecuarias, red vial cantonal, cauces, humedales, corredor biológico, áreas públicas, puntos de referencia, rutas de recolección, aseo de vías, retiros | GeoServer público de corredores.go.cr |
| Servicios WMS del SNIT (en vivo) | ortofotos 1:5000, curvas de nivel, transporte, cauces, edificaciones 1:25 000 y 1:5000, nombres geográficos, UGM del INEC | geos.snitcr.go.cr, geos0.snitcr.go.cr, gestorgeo.inec.go.cr |

Los servicios de Corredores (`https://corredores.go.cr/mapas/publico/wms` y `/ows`) no permiten CORS,
así que sus capas van copiadas como GeoJSON (copia del 29/10/2026). Cada ficha de capa guarda la dirección
del servicio y el nombre de la capa de origen. Las capas del SNIT sí permiten CORS y se piden en vivo.

También se puede consultar como **OGC API – Features** estática en `ogc/` (QGIS: Capa → Añadir capa WFS /
OGC API – Features, con la dirección `https://agricien.github.io/geoportal-costa-rica/ogc`).

## Cuentas de demostración

Cinco cuentas por entidad: `ALIAS_ADMIN`, `ALIAS_PUBLICADOR`, `ALIAS_EDITOR`, `ALIAS_LECTOR01` y
`ALIAS_LECTOR02`. La contraseña es el mismo usuario en mayúsculas; por ejemplo, `municorredores_admin` con
`MUNICORREDORES_ADMIN`. Entidades: MUNICORREDORES, IGN, INEC, ONT, SINAC, MINAE, CNE, MOPT, MEP, CCSS e ICE.
Al entrar se encienden las capas asignadas a la entidad. El acceso de una página estática sólo identifica a quien
entra: no protege datos. Las cuentas nuevas y la bitácora quedan en el navegador de quien las crea.

## Instalar en otro equipo o servidor

Descargue `GeoportalCostaRica-Instalador.exe` desde la página de *Releases* del repositorio y ábralo:

1. **Carpeta y puerto.** Elija dónde instalar (por omisión `C:\GeoportalCostaRica`) y el puerto (8080).
   - *Servidor incluido:* el instalador deja `GeoportalCostaRica.exe`, un servidor web pequeño que
     arranca con Windows y sirve el geoportal en `http://<equipo>:8080/`. Marque «Accesible desde la red»
     para abrir el puerto en el firewall (pide permiso de administrador).
   - *Sólo archivos:* copia el sitio para servirlo con IIS, nginx o Apache. Es un sitio estático: basta
     apuntar la raíz del servidor web a la carpeta. No necesita PHP, Node ni base de datos.
2. **Base de datos (opcional).** Restaura la base `geoportal_costa_rica` (PostGIS) en un PostgreSQL 13 o más
   nuevo con PostGIS 3 instalado: servidor, puerto, usuario administrador y contraseña. Puede crear a la vez un
   usuario de sólo lectura para QGIS. El instalador busca `pg_restore.exe` en `C:\Program Files\PostgreSQL`.
3. **Instalar.** Al terminar abre el geoportal en el navegador.

Para quitarlo: `GeoportalCostaRica.exe --desinstalar` en la carpeta de instalación.

## Base de datos del proyecto

`geoportal_costa_rica` (PostGIS) tiene las tablas completas, sin simplificar:

- `nacional.*`: 21 capas del país;
- `corredores.*`: las 14 capas de la Municipalidad de Corredores;
- `public.capas`: el índice del visor, con fuente, licencia y servicio de cada capa.

## Para regenerar

Las fuentes (`construir.py`, `sitio.py`, el instalador) están fuera de este repositorio, en
`geoportal_costa_rica_fuentes`:

```
"C:\Program Files\QGIS 4.2.2\bin\python-qgis.bat" construir.py base capas volcado
python sitio.py
python generar_ogc.py ..\geoportal-costa-rica https://agricien.github.io/geoportal-costa-rica
instalador\compilar.bat
```

## Licencias

Interfaz: Isocarta, software libre. Datos: los de cada fuente, indicados en la ficha de cada capa. Los
servicios del SNIT y de la Municipalidad de Corredores no declaran licencia.
