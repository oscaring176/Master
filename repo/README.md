# Pipeline de IA para predicción y segmentación de demanda — SITP / Portal Eldorado

Proyecto final (Semana 10) del curso **CIA6041 · Inteligencia Artificial y Gestión de Datos Organizacionales** — Broward International University (BIU).

## Integrantes

Trabajo individual.

- **Oscar Fabián Pedraza García** — IT Technical Leader, BigData/DWH/BI, Tigo Colombia.

## Pregunta de negocio

El Portal Eldorado del SITP/TransMilenio concentra un volumen de salida de personas muy desigual entre sus cinco accesos y a lo largo del día: un solo acceso mueve más de la mitad del flujo total, y existe una franja de la mañana claramente más cargada que el resto. Hoy la asignación de personal, capacidad y señalización se decide con criterio general, sin un mecanismo sistemático que anticipe cuándo y dónde se concentrará la demanda.

Este proyecto responde a esa brecha para el **equipo de operación de la estación** (quien planea turnos y gestiona accesos), con dos preguntas de negocio concretas:

1. **¿Se puede anticipar**, con datos de calendario y de acceso, **si una franja horaria será de alta demanda**, con antelación suficiente para reforzar personal?
2. **¿Qué combinaciones de acceso y franja comparten un perfil de uso similar**, y qué acción operativa distinta amerita cada una?

La primera se responde con un modelo supervisado (árbol de decisión, comparado contra una línea base); la segunda, con una segmentación no supervisada (K-Means).

## Estructura de carpetas

```
repo/
├── README.md                          este archivo
├── Notebook/
│   ├── Notebook_Pipeline_IA_SITP_PortalEldorado.ipynb   notebook completo (ETL + EDA + modelos), ejecutado con salidas visibles
│   └── registro_ejecucion_notebook.txt                  log de una ejecución de punta a punta ("Reiniciar y ejecutar todo"), 0 errores
├── Datos/
│   ├── raw_portal_eldorado_nov2023.csv                   datos crudos, sin modificar
│   └── sitp_portal_eldorado_depurado.csv                 datos depurados (salida del ETL del notebook)
├── Informe/
│   ├── Informe_Tecnico_Final_SITP_PortalEldorado.docx     informe técnico (fuente editable)
│   └── Informe_Tecnico_Final_SITP_PortalEldorado.pdf      informe técnico (entregable, 3 páginas)
├── Presentacion/
│   └── Presentacion_Final_SITP_PortalEldorado.pptx        presentación ejecutiva (6 diapositivas + notas del orador)
└── Dashboard/
    ├── Demanda Operacional - Portal Eldorado.pbix          tablero Power BI (agregar antes de subir — ver nota abajo)
    ├── Procesado_sitp_portal_eldorado_nov2023.xlsx          fuente de datos del tablero (agregar antes de subir)
    └── Capturas_Dashboard_Portal_Eldorado.pdf                PDF con capturas de las vistas principales (agregar antes de subir)
```

> **Nota:** los tres archivos de `dashboard/` viven en el computador de Oscar y no se generaron en este entorno de trabajo — cópialos ahí antes de hacer el `git add` (ver instrucciones de subida más abajo). El PDF de capturas se genera fácil desde Power BI Desktop: **Archivo → Exportar → Exportar a PDF**, que exporta todas las páginas del tablero en un solo archivo.

## Cómo ejecutar el proyecto

### 1. Notebook (Python)

- Abrir `notebook/Notebook_Pipeline_IA_SITP_PortalEldorado.ipynb` en Google Colab o Jupyter.
- Ejecutar todas las celdas en orden (**Entorno de ejecución → Reiniciar y ejecutar todo**, o el equivalente en Jupyter). El notebook lee `datos/raw_portal_eldorado_nov2023.csv`; si se ejecuta en Colab, subir ese archivo a la sesión primero (o ajustar la ruta).
- El notebook regenera `sitp_portal_eldorado_depurado.csv` como parte del ETL — el archivo crudo nunca se modifica.
- Dependencias: `pandas`, `numpy`, `scikit-learn`, `matplotlib` (estándar en Colab).
- Enlace a Colab: **`<< Oscar: pegar aquí el enlace a tu copia en Google Colab >>`**

### 2. Tablero (Power BI)

- Abrir `dashboard/Demanda Operacional - Portal Eldorado.pbix` con Power BI Desktop.
- La fuente de datos es `dashboard/Procesado_sitp_portal_eldorado_nov2023.xlsx` (mismo archivo, en la misma carpeta relativa); si Power BI pide actualizar la ruta del origen de datos, apuntarlo a ese archivo.
- Para verlo sin abrir Power BI: `dashboard/Capturas_Dashboard_Portal_Eldorado.pdf`.

### 3. Informe técnico y presentación

- Se abren directamente: `informe/Informe_Tecnico_Final_SITP_PortalEldorado.pdf` y `presentacion/Presentacion_Final_SITP_PortalEldorado.pptx`.
- La presentación incluye notas del orador (guion de apoyo) en cada diapositiva, visibles en la vista "Notas del orador" de PowerPoint.

## Resumen de resultados

- **Modelo supervisado** (árbol de decisión, profundidad 5): 84,06 % de exactitud vs. 72,18 % de la línea base (+11,88 pp). Recall de 62,9 % en la clase "alta demanda" (193 de 520 picos reales de la semana de prueba no se detectan).
- **Modelo no supervisado** (K-Means, k=4): índice de silueta de 0,361 (estructura moderada, declarada así). Produce 3 grupos accionables (pico matutino entre semana, meseta de tarde/noche, valle de bajo volumen) y 1 grupo residual de 5 combinaciones, tratado explícitamente como ruido.
- **Veredicto de producción:** ambos modelos se recomiendan como apoyo a la decisión humana para el próximo ciclo de programación de turnos, no como automatización autónoma. Condiciones para escalarlos están detalladas en el informe técnico (Sección 5) y en la última diapositiva de la presentación.

## Declaración de uso de herramientas de IA

Este proyecto se desarrolló con apoyo de un asistente de IA (Claude, Anthropic) para: estructurar y depurar el código de ETL/EDA, construir y comparar los modelos de machine learning, generar las visualizaciones, y redactar el informe técnico y la presentación a partir de los resultados obtenidos. Todas las cifras reportadas se calculan en vivo en el notebook (nada se escribió a mano de forma independiente del código que lo produce). La declaración completa está en la Sección 7 del informe técnico.

---

## Cómo subir este repositorio a GitHub (instrucciones para Oscar)

Este proyecto vive en el GitHub de Oscar: **https://github.com/oscaring176/Master**. Como ese repositorio ya contiene el trabajo de otras semanas del curso, la recomendación es subir esta carpeta `repo/` como una subcarpeta propia (por ejemplo `CIA6041-Proyecto-Final/`) dentro de tu repositorio existente, para no mezclar sus archivos con los de otras entregas. Ajusta el nombre si ya tienes una convención distinta.

Pasos, desde tu computador (con Git instalado), una vez tengas esta carpeta `repo/` descargada y con los 3 archivos de `dashboard/` ya copiados adentro:

```bash
# 1. Clonar tu repositorio existente (si aún no lo tienes localmente)
git clone https://github.com/oscaring176/Master.git
cd Master

# 2. Copiar el contenido de esta carpeta "repo/" dentro de una subcarpeta del proyecto
#    (ajusta el nombre si prefieres otro)
mkdir -p CIA6041-Proyecto-Final
cp -r /ruta/donde/descargaste/repo/* CIA6041-Proyecto-Final/

# 3. Agregar, confirmar y subir
git add CIA6041-Proyecto-Final/
git commit -m "Proyecto final SEM10 - Pipeline de IA SITP Portal Eldorado"
git push origin main
```

Después de subirlo:

- Entra a **github.com/oscaring176/Master → Settings → Collaborators** y agrega al Ing. Jhony A. Guzmán H. con su usuario o correo de GitHub, tal como lo mencionaste.
- Copia el enlace a la subcarpeta (por ejemplo `https://github.com/oscaring176/Master/tree/main/CIA6041-Proyecto-Final`) para adjuntarlo en Blackboard.
- Si el repositorio es privado, confirma que la invitación al docente haya sido aceptada antes de la entrega.
