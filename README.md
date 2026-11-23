# Proyecto de automatización de la revisión de expedientes de CGE

Pre clasificador de expedientes de CGE (ARREND). Clasifica cada término, documento
y expediente como **Pasa, No pasa o Revisión**, con el nivel de confianza de cada
lectura y la fuente de cada dato. Alcance vigente: **Guatemala, persona jurídica,
análisis de escrituras y estructura accionaria.**

Todo se trabaja en la nube: **Claude Code y GitHub**.

## Qué hay aquí

| Ruta | Contenido |
|---|---|
| `CLAUDE.md` | Contexto permanente para Claude Code: reglas del proyecto y del repositorio |
| `plugins/clasificador-arrend/` | Fuente única de la habilidad central (orquestador) y del plugin |
| `.claude/skills/clasificador-arrend/` | Espejo de la habilidad: de aquí la carga Claude Code. Se genera con `scripts/sincronizar_habilidad.py` |
| `herramienta/` | Prototipo web pre alfa |
| `docs/` | Reportes por país, reporte técnico, hoja de ruta, pendientes |
| `fuentes/checklists/` | Checklists de Excel extraídos a texto |
| `casos/` | Ignorada por git |
| `scripts/` | Validadores y sincronización |
| `.github/workflows/` | Verificación automática de estructura y privacidad |

## Antes de subir nada: privacidad

Este repositorio es para **código y conocimiento sin datos de clientes**. Aunque sea
privado, no subas expedientes, PDF, capturas, grabaciones, transcripciones, ni
resultados de casos reales (llevan nombres, identificaciones y cifras del cliente).
`.gitignore` bloquea esos tipos de archivo y la verificación automática revisa el
resto. Hay tres puntos de gobernanza abiertos (el contenido viaja al proveedor de la
inteligencia artificial, no hay política de retención y falta la confirmación formal
de Sistemas y Riesgos): ver `docs/pendientes-y-consultas.md`.

## 1. Subirlo a GitHub desde el navegador

1. Descomprime `proyecto-CGE.zip` en tu computadora.
2. Entra a `github.com/ARL-Desarrollos-WEB/proyecto-CGE` y elige **Add file > Upload
   files**.
3. Arrastra **el contenido** de la carpeta descomprimida (no la carpeta contenedora),
   para que `CLAUDE.md` quede en la raíz del repositorio.
4. En **Commit changes**, elige "Create a new branch" y abre la solicitud de cambios,
   o integra a `main` si es la carga inicial.

Si algo no sube:
- **Límite de 100 archivos por carga:** sube en dos lotes (por ejemplo, `plugins` y
  `docs` primero).
- **Carpetas que empiezan con punto** (`.claude-plugin`, `.claude`, `.github`): el
  navegador puede omitirlas. `.claude-plugin/marketplace.json` y
  `.github/workflows/validar.yml` son archivos pequeños: créalos con **Add file >
  Create new file**, escribiendo la ruta completa en el nombre y pegando el contenido.
  La carpeta `.claude/skills/` no hace falta subirla a mano: la primera sesión de
  Claude Code la genera con `python scripts/sincronizar_habilidad.py` (ver el primer
  encargo, abajo).

## 2. Configurar GitHub (una sola vez)

1. **Secreto con los nombres prohibidos.** En **Settings > Secrets and variables >
   Actions > New repository secret**, crea `NOMBRES_PROHIBIDOS` con un nombre por
   línea (clientes, analistas, asesores, representantes, razones sociales). La
   verificación automática la usa en cada push y el contenido nunca queda en el
   repositorio.
2. **Proteger `main`.** En **Settings > Branches** (o **Rules**), exige solicitud de
   cambios y que pase la verificación "Validar estructura y privacidad" antes de
   integrar. Así nada llega a `main` sin pasar la revisión de privacidad.

## 3. Trabajar con Claude Code en la nube

1. En Claude Code en la web, conecta tu cuenta de GitHub y elige el repositorio
   `proyecto-CGE`. La organización necesita la aplicación de Claude para GitHub
   instalada en ese repositorio; probablemente requiera a un administrador.
   El uso de Claude Code en ARREND requiere autorización de la gerencia general.
2. Al abrir la sesión, Claude Code lee `CLAUDE.md` y carga la habilidad
   `clasificador-arrend` desde `.claude/skills/`.
3. Trabaja en ramas y solicitudes de cambios. Después de cambiar la habilidad, ejecuta
   `python scripts/sincronizar_habilidad.py`.
4. **Expedientes reales:** no los subas al repositorio. Qué política aplica a los
   archivos que entran a una sesión en la nube no está confirmado con Sistemas;
   mientras tanto, trabaja con casos ficticios.

### Primer encargo sugerido

> Lee CLAUDE.md, docs/hoja-de-ruta.md y docs/pendientes-y-consultas.md. Si no existe
> la carpeta .claude/skills/clasificador-arrend, ejecuta python
> scripts/sincronizar_habilidad.py. Ejecuta scripts/validar_repo.py y dime qué
> habilidades tienes cargadas. Luego corre los 18 casos de
> plugins/clasificador-arrend/skills/clasificador-arrend/references/casos_prueba.md
> contra la habilidad y dime cuáles no cumplen. Una tarea por vez; trabaja en una
> rama nueva.

### Plugin para otros equipos

`.claude-plugin/marketplace.json` y `plugins/clasificador-arrend/` permiten instalar
el mismo plugin en otro repositorio con `/plugin marketplace add
ARL-Desarrollos-WEB/proyecto-CGE` y `/plugin install clasificador-arrend@arrend-cge`.
**En este repositorio no lo instales además**: duplicaría la habilidad.

## Siguiente en la hoja de ruta

Construir `SH-ESC` (escrituras y estructura accionaria) como habilidad propia,
corregir el orden de los nombres leídos del DPI (E12) y llevar la clasificación y la
escala de confianza a la herramienta web. Detalle en `docs/hoja-de-ruta.md`.

## Estado

| Componente | Estado |
|---|---|
| Habilidad central del clasificador | Versión 0.2, validada; casos de prueba escritos, no ejecutados contra el modelo |
| Carga de la habilidad en una sesión en la nube | No verificada |
| Habilidad de escrituras (SH-ESC) | Definida como referencia; falta extraerla como habilidad |
| Herramienta web | Pre alfa; sin la clasificación ni la escala de confianza |
| Checklists | Sin versión ni vigencia confirmadas con CGE |
