# Clientes Nuevos · Supertiendas Cañaveral

Tablero de seguimiento del registro de clientes nuevos (Habeas Data) y su cumplimiento
frente al presupuesto, por sede, cluster y período.

## Publicar en GitHub Pages

1. Crea un repositorio nuevo (por ejemplo `clientes-nuevos`).
2. Sube el archivo `index.html` a la raíz del repositorio.
3. Entra a **Settings → Pages**.
4. En *Source* elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`. Guarda.
5. A los pocos minutos la app queda disponible en
   `https://<usuario>.github.io/<repositorio>/`

No hay nada que compilar ni instalar: es un solo archivo HTML.

## Cómo se usa

**Datos** (botón arriba a la derecha)

- **Habeas Data**: la base completa de clientes. Cada vez que se sube **reemplaza** la
  anterior, así que hay que subir siempre la versión más reciente. Solo se cuentan los
  registros marcados como `NUEVO`; los `ACTUALIZADO` se descartan solos.
- **Reportes diarios**: los archivos que llegan día a día ya sumados por sede
  (`reporte-registro-AAAA-MM-DD.xlsx`). Se acumulan y se suman a la base. La fecha se toma
  del nombre del archivo. Si se sube uno repetido, se ignora.
- **Memoria**: un archivo `.json` con todo lo acumulado (reportes, presupuesto y sedes).
  Se carga al empezar y se guarda al terminar. Sirve para pasar la información a otro
  computador o para respaldarla.

> Si los reportes diarios ya vienen incluidos dentro del Habeas Data, hay que **desmarcar**
> la casilla *«Sumar los reportes diarios»*; de lo contrario los números salen más altos de
> lo real. Para saberlo, comparar un mismo mes con y sin la marca.

**Ajustes** (el engranaje)

- **Presupuesto**: se digita únicamente el objetivo **mensual** por sede. El diario, el
  semanal y el anual se calculan a partir de ese. Se puede descargar en Excel, editarlo y
  volverlo a subir (columnas `Mes`, `Sede`, `Objetivo`; acepta `.xlsx`, `.csv` y `.txt`).
- **Sedes**: traduce el nombre con que llega cada sede en los archivos al nombre real, y le
  asigna su cluster. Las sedes con cluster *(no incluir)* quedan fuera de todos los conteos.

**Tablero**

- Vistas **Anual / Mensual / Semanal / Día**, con filtro por cluster y por sede.
- Botón **Exportar a Excel** de la tabla de cumplimiento.
- Botón **Compartir** (cámara): descarga la vista como imagen PNG, la manda a imprimir o
  guardar como PDF, o genera el PDF directamente.
- Botón **Guardar**: descarga la memoria.

## Cómo se calcula el cumplimiento

| Escala   | Objetivo |
|----------|----------|
| Diario   | objetivo mensual ÷ días del mes |
| Semanal  | objetivo diario × días de esa semana que caen dentro del mes (la semana va de domingo a sábado) |
| Mensual  | objetivo del mes completo |
| Anual    | suma de los 12 objetivos mensuales |

**Faltan** = objetivo − inscritos (nunca es negativo).

Estados: **CUMPLE** desde 100% · **EN RIESGO** entre 85% y 99% · **NO CUMPLE** por debajo
de 85% · **SIN OBJETIVO** cuando la sede no tiene presupuesto en ese período.

## Notas técnicas

- Los archivos se procesan en el navegador; los datos personales de la base no salen del
  equipo ni se guardan: solo se conserva el conteo por fecha y sede.
- Lo cargado queda guardado en el navegador del equipo, así que al refrescar no se pierde.
  Aun así conviene guardar la memoria para respaldar o pasar a otro computador.
- La fecha oficial de cada registro es la columna `updatedAt`.
- React, SheetJS, html2canvas y jsPDF se cargan desde cdnjs: hace falta conexión a internet.
  Sin conexión el tablero no abre.
- Si la descarga del PDF falla en algún equipo, usar **Imprimir o guardar PDF**, que no
  depende de librerías externas.
