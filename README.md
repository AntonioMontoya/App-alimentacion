# Mi Diario · Alimentación

Diario nutricional personal en español. Diseñado para registrar lo que has comido desde el móvil o el ordenador, con controles táctiles, calendario y modo claro/oscuro.

**[Abrir Mi Diario · mi-diario-nutricional.wintryocean4.chatgpt.site](https://mi-diario-nutricional.wintryocean4.chatgpt.site)**

El enlace abre la app privada publicada. Inicia sesión con la misma cuenta de ChatGPT en móvil y ordenador. Después de subir este proyecto a GitHub, puedes abrir la app cada día desde este README.

## Qué puedes hacer

- Crear únicamente las comidas que necesitas: Desayuno 1, Desayuno 2, Comida, Cena o cualquier nombre propio. Agruparlas en seis categorías para comparar estadísticas.
- Añadir platos manualmente con kcal, carbohidratos, proteínas y grasas; un dato vacío se conserva como desconocido, no como cero.
- Subir una foto del alimento o de toda la comida. Se comprime a WebP, hasta 960 px y 200 KB como máximo (objetivo 150 KB), antes de guardarse. No hay reconocimiento automático de nutrientes. Después de sincronizar, puedes borrar el original del móvil.
- Buscar 158 alimentos básicos de USDA FoodData Central con valores por 100 g, diferenciando crudo y cocinado. Consultar la fuente original, guardar favoritos y alimentos propios por g, ml o ración.
- Guardar comidas completas como presets, reutilizarlas y ajustar cantidades. Las entradas conservan una copia de la ficha nutricional: cambiar un alimento o un preset no reescribe tu historial.
- Consultar calendario, totales diarios y medias semanales, mensuales o de un intervalo. Comparar por categoría o nombre de comida y alternar media por día/ocasión.
- Marcar explícitamente los días completos. Las medias usan estos días por defecto; el filtro de días registrados incluye también parciales. Las fechas vacías y futuras no se interpretan como ingestas de cero.
- Sincronizar entre dispositivos, conservar cambios pendientes sin conexión, resolver conflictos sin sobrescrituras silenciosas, exportar CSV y descargar/restaurar copias ZIP con fotos.
- Mantener una copia adicional automática en un repositorio privado de GitHub una vez conectado.

## Acceso y guardado

La versión alojada usa acceso privado con la cuenta de ChatGPT propietaria del sitio. Abre el mismo enlace con la misma cuenta en ambos dispositivos. El diario y las fotos se guardan en almacenamiento remoto; IndexedDB es una caché y una cola para cambios pendientes, no la única copia.

GitHub Pages por sí solo no ofrece un servidor privado para recibir fotos ni guardar una credencial. Por eso el sitio completo usa un Worker y almacenamiento R2. El código se puede guardar en GitHub y un enlace en el README permite abrir la app cada día.

### Copia privada en GitHub

1. Crea `mi-diario-datos` como repositorio **privado**, inicializado con un README.
2. Crea un token de acceso personal de alcance limitado, seleccionado únicamente ese repositorio y el permiso **Contents: Read and write**.
3. En la app, abre **Ajustes → Copia en GitHub**, introduce `usuario/mi-diario-datos` y la credencial, y conecta.

La credencial viaja al servidor por HTTPS y se guarda cifrada con AES-GCM; no se incluye en el código, las exportaciones ni el almacenamiento del navegador. Cada guardado programa una copia de `diario.json` y las fotos WebP mediante un commit atómico en GitHub. Si GitHub falla, el diario remoto sigue guardado y se muestra el error para reintentar. La copia de GitHub es adicional: desconectarla no borra el diario.

El acceso de GitHub disponible durante la creación de este proyecto rechazó crear repositorios (HTTP 403), de modo que la entrega incluye el proyecto descargable. No se ha configurado todavía ninguna copia de datos en GitHub.

## Desarrollo local

Requiere Node.js 22 o superior.

```sh
npm ci
npm run build
npm run dev:api
```

Abre `http://localhost:8787`. El entorno `preview` utiliza R2 local y habilita explícitamente `LOCAL_PREVIEW=1`; este modo no debe utilizarse en producción. Para desarrollo con recarga inmediata, conserva la API y ejecuta en otra terminal:

```sh
npm run dev
```

Abre `http://localhost:5173`. Vite envía las solicitudes `/api` al servidor local.

```sh
npm test
npm run build
```

Consulta [Arquitectura y despliegue](docs/arquitectura.md) y [Comprobaciones](docs/verificacion.md).

## Fuentes y licencias

- Catálogo: [USDA FoodData Central, SR Legacy](https://fdc.nal.usda.gov/download-datasets/) (publicado en 2019, datos SR 2018), [CC0](https://fdc.nal.usda.gov/data-documentation.html). Traducción de nombres y selección de 158 fichas para esta app. Los valores son genéricos: la etiqueta del producto concreto tiene prioridad. Se incluyen FDC ID y descripción original en cada ficha.
- Regeneración: descarga y descomprime [el CSV oficial](https://fdc.nal.usda.gov/fdc-datasets/FoodData_Central_sr_legacy_food_csv_2018-04.zip) y ejecuta `python scripts/build-catalog.py /ruta/al/directorio/CSV`.
- Foto de la pantalla vacía: [Alexandru Acea en Unsplash](https://unsplash.com/photos/oatmeal-in-white-bowl-Vk044I3w1gI), [licencia Unsplash](https://unsplash.com/license). Es una imagen de referencia, no un registro del usuario.
- Investigación y especificación original: [dossier](docs/dossier.md).
- Código del proyecto: MIT. Las dependencias conservan sus propias licencias.
