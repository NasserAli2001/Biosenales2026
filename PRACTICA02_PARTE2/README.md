# Práctica 02 – Análisis estadístico de señales

**Bioseñales y Sistemas – Bioingeniería, Facultad de Ingeniería**

**Juan Felipe Pereira y Nasser Salam**

## Parte 2.5: Comparación estadística entre bradicardia sinusal y fibrilación auricular

Este README es el resumen de la práctica. El desarrollo completo, con el código, las gráficas y el análisis, está en el notebook `Entrega2_Arritmias_Chapman.ipynb`.

### Resumen

Trabajamos con la base de ECG de 12 derivaciones de Chapman University y el Shaoxing People's Hospital (Zheng et al., 2020). El objetivo era explorar la base y comparar estadísticamente dos ritmos: bradicardia sinusal (SB) y fibrilación auricular (AFIB), usando las señales filtradas de `ECGDataDenoised`.

**Exploración.** La base tiene 10646 registros, uno por paciente, con 12 derivaciones, 500 Hz y 10 s por registro. Hay 11 ritmos y están bastante desbalanceados. En el camino encontramos algunos detalles en los archivos: la irregularidad sinusal aparece como SI en `RhythmNames.xlsx` pero como SA en `Diagnostics.xlsx`, y el artículo reporta un número de pacientes con AFIB que no coincide con los datos. Los dejamos documentados en el notebook.

**Registros de ejemplo.** Graficamos un registro de cada ritmo y analizamos morfología, amplitud, frecuencia cardíaca y regularidad. Detectamos los picos R con `find_peaks` sobre la derivada al cuadrado de la derivación II. En el ejemplo, la SB se ve lenta y regular, con ondas P, y la AFIB no tiene ondas P y sus intervalos RR son irregulares.

**Análisis estadístico.** A cada uno de los 5669 archivos de SB y AFIB le calculamos seis características sobre la derivación II: media, desviación estándar, máximo, mínimo, frecuencia cardíaca (FC) y RMS. Descartamos un solo archivo de SB, porque el filtrado venía cortado (1926 muestras en vez de 5000). Así quedaron 3888 registros de SB y 1780 de AFIB. Revisamos también la FC calculada contra la frecuencia ventricular que trae `Diagnostics.xlsx`.

Luego comprobamos los supuestos de la prueba t. La normalidad (D'Agostino-Pearson y QQ plots) y la homocedasticidad (Levene) no se cumplieron en ninguna característica, así que usamos la U de Mann-Whitney con α = 0.05. Como tamaño de efecto usamos la correlación biserial de rangos (r). Además revisamos las decisiones con la corrección de Bonferroni (α = 0.0083).

### Resultados principales

| Característica | Mediana SB | Mediana AFIB | r | Magnitud |
|---|---|---|---|---|
| Frecuencia cardíaca (lpm) | 55.98 | 91.24 | −0.936 | grande |
| Media (µV) | 32.57 | 22.96 | 0.260 | pequeño |
| Valor mínimo (µV) | −144.38 | −188.72 | 0.170 | pequeño |
| Valor máximo (µV) | 807.68 | 727.84 | 0.107 | pequeño |
| Desviación estándar (µV) | 110.50 | 115.14 | −0.075 | muy pequeño |
| Valor RMS (µV) | 115.72 | 118.24 | −0.052 | muy pequeño |

Las seis diferencias salieron significativas, también con Bonferroni: el p-valor más alto fue 1.666e-03, del RMS. Pero con miles de registros casi cualquier diferencia sale significativa, así que para interpretar nos fijamos sobre todo en el tamaño del efecto.

### Conclusiones

- La FC es la única característica que separa bien los dos grupos. Los registros de SB y AFIB casi no se traslapan, lo cual es lógico por la definición de cada arritmia.
- La media, el máximo y el mínimo cambian un poco entre grupos. La desviación estándar y el RMS prácticamente no cambian. Estas medidas describen la amplitud de la señal, pero dicen poco sobre el ritmo.
- Limitaciones: los pacientes con AFIB son mayores que los de SB, muchos tienen otras condiciones cardiacas y solo analizamos la derivación II.

### Datos

Las señales de la base (carpetas `ECGData` y `ECGDataDenoised`, unos 8.5 GB) no se subieron al repositorio porque superan el tamaño que permite GitHub.

## Referencia principal

J. Zheng, J. Zhang, S. Danioko, H. Yao, H. Guo y C. Rakovski, "A 12-lead electrocardiogram database for arrhythmia research covering more than 10,000 patients," *Scientific Data*, vol. 7, art. 48, 2020. doi: 10.1038/s41597-020-0386-x.

Las demás referencias están al final del notebook.
