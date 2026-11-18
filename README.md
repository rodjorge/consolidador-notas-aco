# Consolidador de notas · Arquitectura de Computadoras (UTN FRC)

Página para unir las notas de **Moodle** (microevaluaciones, parciales) con las de los **exámenes en papel** (Excel), calcular la **condición académica** de cada estudiante y exportar las planillas en Excel.

**Usarla:** https://rodjorge.github.io/consolidador-notas-aco/

## Privacidad: las notas no salen de tu computadora

- La página es un único archivo HTML. Los Excel se leen **en tu navegador**: no se suben a GitHub ni a ningún otro servidor.
- La página incluye una política de seguridad (`Content-Security-Policy`) que **le prohíbe al navegador enviar datos** a cualquier servidor. Lo único que se descarga al abrirla es la librería [SheetJS](https://sheetjs.com) para leer y escribir Excel.
- Lo podés comprobar: abrí las herramientas del navegador (F12) → pestaña **Red**, y cargá tus archivos. No sale ningún pedido.
- Lo que se guarda en tu navegador es sólo la **configuración** de cada comisión (archivos, columnas y reglas), nunca las notas.

> ⚠ Este repositorio es público. **Nunca** subas planillas con datos reales. El `.gitignore` excluye todas las planillas salvo las de `ejemplos/`, que tienen datos inventados.

## Cómo se usa

1. **Comisión:** escribí el nombre (por ej. `1K1`). La configuración se guarda sola. Con *Duplicar* la copiás para otra comisión, y con *Exportar/Importar configuración* (.json) la compartís con otros docentes.
2. **Archivos:** vienen tres por defecto, *Microevaluaciones*, *Parcial 1* y *Parcial 2*, cada uno con su color; se pueden renombrar, quitar o agregar más. Cargá el export de Moodle (*Calificaciones → Exportar → Hoja de cálculo Excel*) y los Excel de los exámenes en papel. En cada uno indicá la columna del **legajo** (con ella se cruzan los archivos) y la del **apellido y nombre**, que pueden estar juntos o en dos columnas. El legajo se cruza aunque esté escrito con punto, coma o espacios.
3. **Evaluaciones:** configurá la **cantidad de microevaluaciones** (varía por curso) y la **cantidad de parciales** (en la cátedra, 2). Cada parcial tiene un **recuperatorio opcional**: el estudiante puede recuperar ambos parciales, uno o ninguno. Para cada fila elegí de qué archivo y qué columna sale la nota; la fila toma el color de su archivo. *Autodetectar* asigna solas las columnas que reconoce. El tipo de nota de cada una puede ser:
   - *Nota Moodle*: puntaje de un cuestionario (sobre 100, 10, etc.); se pasa a porcentaje y de ahí a nota con la escala.
   - *Nota final*: la nota ya está puesta (por ejemplo, en el Excel del examen en papel) y se usa tal cual, sin convertir.
4. **Reglas:** vienen cargadas las de la cátedra (ver abajo). Se pueden cambiar por comisión.
5. **Resultados:** muestra cuántos estudiantes quedan en cada condición, una observación por estudiante y avisos para revisar (por ejemplo, estudiantes que faltan en algún archivo o legajos mal cargados).
6. **Exportar:**
   - *Planilla para el sistema*: con las columnas, el orden y los títulos que elijas; en .xlsx o .csv.
   - *Detalle para docentes*: todas las notas, más las reglas usadas y los avisos.
   - *Un solo Excel con todo*: ambas cosas en el mismo archivo.

## Reglas de la cátedra (valores por defecto)

**Escala de notas** (porcentaje → nota; el porcentaje se redondea al entero):

| Nota | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| desde | 0 % | 30 % | 50 % | 55 % | 58 % | 60 % | 69 % | 78 % | 87 % | 96 % |

- Parcial y microevaluación **aprobados con nota 4** o más (55 % o más).
- Entre un parcial y su recuperatorio **queda siempre la nota mayor**.
- El % de micros aprobadas se calcula sobre la cantidad de micros configurada; una micro no rendida no está aprobada y, en el promedio de micros, cuenta como **1** (la nota mínima).

**Condición**, evaluada en este orden:

| # | Situación | Condición |
|---|---|---|
| 1 | Sin ninguna nota registrada | No Cursó |
| 2 | Falta la nota de algún parcial | Abandonó |
| 3 | Algún parcial con menos de 4 | Libre |
| 4 | Ambos parciales con 7 o más y al menos el 75 % de las micros aprobadas | Aprobación Directa |
| 5 | El resto (parciales aprobados) | Regular |

La nota final de Aprobación Directa es el promedio de los parciales redondeado al entero.

## Probarla sin datos reales

En [`ejemplos/`](ejemplos/) hay dos planillas con **datos ficticios**: un export de Moodle y un Excel de 1er parcial en papel.
