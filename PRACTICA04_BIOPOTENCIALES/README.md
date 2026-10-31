# Práctica 04: Introducción a los biopotenciales y adquisición de señales ECG y EMG

**Bioseñales y Sistemas – Bioingeniería, Facultad de Ingeniería**

**Juan Felipe Pereira y Nasser Salam**

Este README es el resumen de la práctica. El desarrollo completo, con el código, las gráficas y el análisis, está en el notebook "Informe_ECG_EMG_Biosenales.ipynb".

### Resumen

Registramos un electrocardiograma (ECG) en reposo con el triángulo de Einthoven y una electromiografía de superficie (sEMG) del antebrazo con contracciones repetidas. Usamos el módulo SHIELD-EKG-EMG de Olimex sobre un Arduino UNO y pasamos los datos al computador con los códigos de adquisición entregados por la profesora, 5000 muestras por registro.

**Teoría (puntos 5.1 a 5.3).** Hicimos el esquema de las etapas del registro de biopotenciales, una tabla con las fuentes de ruido (estáticas o variantes y cómo se controlan) y la discusión del acoplamiento piel-electrodo.

**Señales.** La frecuencia de muestreo no quedó registrada, así que la estimamos con el pico de la red de 60 Hz que aparece doblado en el espectro del sEMG (unos 97 Hz). El ECG lo filtramos con un pasa-banda de 0.5 a 35 Hz para los detectores. Al sEMG le aplicamos un pasa-altas, rectificación y envolvente para ubicar las contracciones.

**Variabilidad de la frecuencia cardíaca (HRV).** Detectamos los picos R con "ecg_peaks" de NeuroKit y con los detectores de Pan-Tompkins y de dos promedios de Elgendi de "py-ecg-detectors", y los comparamos contra unos picos de referencia revisados a ojo. Con el mejor detector calculamos los intervalos RR, el tacograma y los índices en el dominio del tiempo con "get_time_domain_features" de "hrvanalysis".

**Aplicaciones clínicas.** Consultamos tres aplicaciones del ECG y tres del EMG, con el tipo de registro y el procesamiento de cada una.

### Resultados principales

- El detector de dos promedios de Elgendi fue el mejor: encontró todos los picos R sin picos de más y los ubicó cerca del R. NeuroKit falló en los bordes del registro y Pan-Tompkins marcó los picos corridos.
- Los valores de HRV son razonables para una persona sana, pero el registro dura menos de un minuto y la frecuencia de muestreo es baja, así que solo sirven para ilustrar el procedimiento.
- En el sEMG las contracciones se distinguen claramente del reposo con un procesamiento sencillo.
- La ganancia del shield era muy alta y los picos R del ECG se saturaron en 5 V; para contar latidos no fue un problema.

### Archivos

- "Informe_ECG_EMG_Biosenales.ipynb": notebook con todo el informe.
- "lab_bioseñales/EMG/EMG_NASSER.txt": datos del sEMG.
- "lab_bioseñales/EMG/Figure_1.png": gráfica del sEMG guardada en la práctica.

Las referencias están al final del notebook.
