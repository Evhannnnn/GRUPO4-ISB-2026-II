# Filtros digitales: FIR, IIR y Transformada Z

**Curso:** Introducción a Señales Biomédicas
**UPCH — 2026-II | Grupo 4**

---

# PARTE I. FUNDAMENTOS TEÓRICOS

## 1. Filtrado digital

Un filtro digital es un sistema lineal e invariante en el tiempo que modifica de forma selectiva el contenido frecuencial de una señal discreta. Opera sobre las muestras mediante una ecuación en diferencias, donde la salida actual se calcula a partir de las entradas presentes y pasadas y, en algunos casos, de las salidas anteriores.

Frente al filtrado analógico ofrece reproducibilidad exacta e independencia respecto de las tolerancias de los componentes. Su limitación es que solo puede actuar sobre frecuencias inferiores a la mitad de la frecuencia de muestreo, y no puede corregir el solapamiento espectral introducido durante la conversión analógica a digital.

## 2. Dominios de análisis

Una señal biomédica admite tres perspectivas complementarias. El dominio temporal muestra cómo varía la amplitud e identifica eventos como los complejos QRS. El dominio frecuencial, obtenido mediante la Transformada de Fourier, revela qué componentes integran la señal y permite distinguir la energía fisiológica del ruido. El dominio conjunto de tiempo y frecuencia, obtenido mediante la Transformada de Fourier de tiempo corto, indica en qué instante aparece cada componente espectral.

El tamaño de ventana en este último caso establece un compromiso inevitable. Una ventana breve localiza con precisión los eventos en el tiempo pero distingue mal frecuencias próximas; una ventana extensa discrimina bien las frecuencias pero difumina el instante en que ocurren. Es una manifestación del principio de incertidumbre y no puede eludirse mediante ajustes del algoritmo.

## 3. Transformada Z, polos y ceros

La Transformada Z convierte la ecuación en diferencias en un cociente algebraico de polinomios denominado función de transferencia. Las raíces del numerador son los **ceros** y determinan las frecuencias que el sistema atenúa. Las raíces del denominador son los **polos** y establecen las resonancias, además de condicionar la estabilidad.

El criterio de estabilidad es directo: un sistema causal resulta estable si todos sus polos se ubican dentro del círculo unitario del plano complejo. Los ceros no intervienen en la estabilidad, únicamente modelan la forma de la respuesta en frecuencia.

La Transformada de Fourier de tiempo discreto es un caso particular de la Transformada Z evaluada sobre el círculo unitario. Por ello la respuesta en frecuencia se obtiene recorriendo dicho círculo, y la posición angular de cada polo y de cada cero indica la frecuencia sobre la que ejerce mayor influencia.

## 4. Filtros FIR e IIR

Lo que separa a ambos tipos es la presencia de realimentación.

Los filtros **FIR**, de respuesta al impulso finita, calculan la salida como suma ponderada exclusivamente de las entradas. Al carecer de realimentación todos sus polos se sitúan en el origen, por lo que son incondicionalmente estables. Con coeficientes simétricos la fase resulta perfectamente lineal, lo que equivale a un retardo constante para todas las frecuencias.

Los filtros **IIR**, de respuesta al impulso infinita, incorporan salidas previas en el cálculo. Esta realimentación genera resonancias y pendientes pronunciadas con muy pocos coeficientes, pero sus polos ocupan posiciones arbitrarias y su estabilidad debe verificarse.

| Característica | FIR | IIR |
|---|---|---|
| Realimentación | No | Sí |
| Estabilidad | Incondicional | Debe analizarse |
| Fase lineal | Directa | No, salvo filtrado bidireccional |
| Orden requerido | Alto | Bajo |
| Coste computacional | Mayor | Menor |

Se prefiere FIR cuando la posición temporal de los eventos resulta crítica, e IIR cuando prima la eficiencia computacional.

## 5. Respuesta en frecuencia y orden

La respuesta de magnitud expresa la atenuación aplicada a cada frecuencia; la respuesta de fase indica el desplazamiento temporal introducido, y su derivada, el retardo de grupo, resulta constante solo en filtros de fase lineal.

La especificación se formula sobre tres regiones: la banda de paso, que se conserva; la banda de rechazo, que se atenúa; y la banda de transición entre ambas, cuyo ancho determina el orden necesario.

El **orden** equivale al número de polos y controla la pendiente de caída, aproximadamente veinte decibelios por década por cada unidad. Incrementarlo eleva el coste computacional, prolonga el transitorio inicial, acentúa la distorsión de fase y compromete la estabilidad numérica. El criterio correcto consiste en calcular el orden mínimo que satisface las especificaciones.

## 6. Diseño FIR mediante ventanas

La respuesta al impulso ideal es infinita y no causal. El método de ventanas la trunca a longitud finita y la multiplica por una función que suaviza los extremos. El truncamiento abrupto origina el fenómeno de Gibbs, con oscilaciones en la banda de paso y lóbulos laterales elevados en la banda de rechazo.

Cada ventana desplaza el compromiso entre atenuación de los lóbulos laterales y ancho de la banda de transición. La rectangular ofrece la transición más estrecha con la peor atenuación; Blackman, la situación inversa; Hamming y Hann ocupan posiciones intermedias, y Kaiser incorpora un parámetro ajustable que permite fijar el punto de equilibrio.

## 7. Familias clásicas de filtros IIR

| Familia | Rizado en banda de paso | Rizado en banda de rechazo | Transición |
|---|---|---|---|
| Butterworth | No | No | Lenta |
| Chebyshev tipo I | Sí | No | Rápida |
| Chebyshev tipo II | No | Sí | Rápida |
| Elíptico | Sí | Sí | Mínima |
| Bessel | No | No | Muy lenta |

El **Butterworth** presenta una banda de paso máximamente plana y es la elección habitual en señales biomédicas, donde la amplitud suele ser la variable de interés. El rizado de un Chebyshev o de un filtro elíptico introduciría una distorsión de amplitud dependiente de la frecuencia, que alteraría medidas como la amplitud del complejo QRS. Se acepta una transición menos abrupta a cambio de preservar la magnitud.

## 8. Biquads y secciones de segundo orden

Un biquad es una sección elemental de segundo orden, con dos polos y dos ceros. Cualquier filtro IIR de orden superior puede factorizarse en una cascada de biquads.

El motivo es práctico. En la forma directa, los coeficientes de un filtro de orden elevado abarcan rangos numéricos muy amplios; al implementarlos con precisión finita, los errores de redondeo desplazan la posición efectiva de los polos y algunos pueden cruzar el círculo unitario. Un filtro estable en el diseño se vuelve inestable al ejecutarse. La factorización limita cada bloque a coeficientes de magnitud moderada y conserva la respuesta global. Se emplea a partir del orden cuatro.

## 9. Transformación bilineal

Permite convertir un filtro analógico, cuyo diseño está teóricamente consolidado, en su equivalente digital. Lo que la hace útil es que mapea el semiplano izquierdo del plano de Laplace al interior del círculo unitario, lo que garantiza que un filtro analógico estable produzca un filtro digital estable.

Introduce una distorsión del eje de frecuencias denominada warping: el eje analógico, de extensión infinita, se comprime en un rango finito, de modo que la correspondencia es aproximadamente lineal en frecuencias bajas y se distorsiona al aproximarse al límite de Nyquist. La compensación, llamada prewarping, predistorsiona la frecuencia analógica antes de la conversión para que el corte resultante coincida con el especificado. Las funciones de SciPy lo aplican de forma automática cuando se indica la frecuencia de muestreo.

---

# PARTE II. DESARROLLO DE LOS LABORATORIOS

Las tres sesiones prácticas siguieron una progresión deliberada: primero obtener y observar una señal, después analizarla en frecuencia y, finalmente, modificarla mediante filtros. Cada laboratorio incorpora un dominio de análisis adicional y un nivel mayor de decisión por parte del estudiante.

| | Laboratorio 01 | Laboratorio 02 | Laboratorio 03 |
|---|---|---|---|
| Objetivo | Adquirir y observar | Analizar el contenido espectral | Diseñar y validar filtros |
| Origen de la señal | Registro real de PhysioNet | Tres registros reales de PhysioNet | Señal sintética con referencia limpia |
| Dominio de trabajo | Temporal | Frecuencial y tiempo frecuencia | Los tres, de forma integrada |
| Herramienta central | WFDB | FFT y STFT | Diseño FIR e IIR |
| Decisión del estudiante | Segmento y canal | Tamaño de ventana | Tipo de filtro, corte y orden |
| Criterio de éxito | Reconocer la morfología | Distinguir señal de artefacto | Conservar la información fisiológica |

## 10. Laboratorio 01. Adquisición de registros reales

Se trabajó con la base MIT-BIH Arrhythmia de PhysioNet mediante la librería WFDB. Un registro se identifica siempre por la combinación de base de datos y número, dado que el mismo número puede corresponder a señales distintas en bases diferentes.

```python
record = wfdb.rdrecord(RECORD, pn_dir=DATABASE)
signal = record.p_signal[:, CHANNEL]
t = np.arange(len(signal)) / fs
```

En la estructura `p_signal` las filas corresponden a muestras y las columnas a canales. El eje temporal no se almacena en el archivo: se reconstruye a partir de la frecuencia de muestreo, ya que el instante asociado a la muestra n equivale a n dividido entre fs.

El registro analizado presentó una frecuencia de muestreo de 360 Hz y dos canales en milivoltios. Un segmento de diez segundos equivale por tanto a 3600 muestras.

La señal se representó desde cuatro perspectivas: el registro completo, un segmento ampliado, el histograma de amplitudes y la representación discreta de muestras individuales. El histograma describe la distribución de amplitudes pero elimina toda referencia temporal, lo que ilustra que ninguna representación aislada agota la información de la señal.

Como ejercicio complementario se exportó un segmento a formato de audio, procedimiento que exigió eliminar la componente continua, normalizar por el valor absoluto máximo y convertir a representación entera. El archivo resultante conserva la forma de onda y la separación temporal entre muestras, pero el electrocardiograma no es una señal acústica: escucharlo es una representación, no una medición.

## 11. Laboratorio 02. Análisis espectral y tiempo frecuencia

Se compararon tres registros de la base Normal Sinus Rhythm, muestreados a 128 Hz, lo que sitúa la frecuencia máxima observable en 64 Hz.

El espectro se calculó con y sin componente continua:

```python
spectrum_ac = np.fft.rfft(x - np.mean(x))
frequencies = np.fft.rfftfreq(n, d=1/fs)
```

La componente continua corresponde al valor medio de la señal y aparece como un pico en frecuencia cero cuya magnitud puede enmascarar por completo el contenido fisiológico. Su eliminación permite visualizar la banda de interés en escala adecuada. Por la misma razón, la búsqueda de la frecuencia dominante excluye el primer índice del espectro.

El análisis conjunto de tiempo y frecuencia se obtuvo segmentando la señal en ventanas sucesivas:

```python
f, t, zxx = signal.stft(x, fs=fs, nperseg=NPERSEG)
```

El parámetro `nperseg` es donde aparece el compromiso descrito en la teoría. Con 256 muestras la ventana abarca dos segundos y ofrece una resolución frecuencial de 0.5 Hz; con 32 muestras la ventana se reduce a un cuarto de segundo y la resolución frecuencial cae a 4 Hz, ganando en cambio precisión temporal. Se utilizó la ventana corta en el registro que presentaba eventos transitorios, con el fin de localizarlos con mayor exactitud.

En los espectrogramas los complejos QRS se manifestaron como bandas verticales periódicas de energía, dado que concentran las componentes de mayor frecuencia del electrocardiograma. El registro con deriva de línea base mostró la mayor variabilidad espectral, apreciable en el dominio conjunto pero no evidente en el espectro global.

## 12. Laboratorio 03. Diseño y validación de filtros

Se partió de una señal electrocardiográfica sintética generada con NeuroKit2, muestreada a 250 Hz durante diez segundos. Trabajar con una referencia limpia permite cuantificar posteriormente el efecto del filtrado.

**Caracterización previa.** El espectro mostró las componentes dominantes por debajo de 10 Hz, correspondientes a los armónicos de la frecuencia cardíaca, con una energía significativa que decae entre 20 y 25 Hz. Este análisis es el que fundamenta la elección de la frecuencia de corte, que se sitúa en la banda de transición entre la energía fisiológica y el ruido, nunca por criterio arbitrario.

**Diseño comparativo.** Se implementaron un filtro FIR y un filtro IIR con la misma frecuencia de corte:

```python
b_fir = signal.firwin(numtaps=101, cutoff=40.0, fs=fs)
sos_iir = signal.butter(N=4, Wn=40.0, btype='lowpass', fs=fs, output='sos')
```

La comparación de ambas respuestas en frecuencia confirmó lo previsto teóricamente: el filtro IIR de orden cuatro alcanza una atenuación equivalente a la del FIR de 101 coeficientes. El argumento `output='sos'` responde a la necesidad de estabilidad numérica descrita anteriormente.

Ambos se aplicaron con filtrado bidireccional mediante `filtfilt` y `sosfiltfilt`, que recorren la señal en los dos sentidos y cancelan la distorsión de fase.

**Caso con interferencia.** Se añadió a la señal una sinusoide de amplitud reducida cuya frecuencia no se proporcionaba como dato. La FFT de la señal contaminada reveló un pico aislado en 35 Hz, ausente en el espectro original.

El corte se fijó por debajo de esa frecuencia y por encima de la banda fisiológica principal. Los filtros del ejercicio anterior, con corte en 40 Hz, no habrían servido aquí: la interferencia de 35 Hz cae dentro de su banda de paso. Un filtro es adecuado o no según el problema que se le plantee.

**Validación.** Se combinaron cuatro criterios: comparación de la morfología temporal, superposición de los espectros antes y después del filtrado, cálculo de métricas cuantitativas de error y verificación de que los picos R mantenían su posición temporal. El error cuadrático medio respecto de la referencia limpia resultó del orden de diez elevado a menos cinco, lo que indica ausencia de distorsión apreciable.

**Errores de diseño.** Se examinaron experimentalmente dos casos. Una frecuencia de corte excesivamente baja, del orden de 5 Hz, aplana y ensancha el complejo QRS al eliminar los armónicos responsables de sus flancos abruptos, con la consiguiente pérdida de información diagnóstica. El uso de filtrado causal mediante `sosfilt` en lugar de la versión bidireccional desplaza temporalmente la señal, ya que un filtro IIR aplicado en un solo sentido presenta retardo de grupo dependiente de la frecuencia; esto compromete la medición de intervalos como el QT o el tiempo entre picos R.

---

## 13. Comparación entre los tres laboratorios

Cada sesión responde una pregunta distinta sobre la misma señal.

En el **Laboratorio 01** la pregunta es qué contiene el registro. El trabajo es descriptivo: leer los metadatos, reconstruir el eje temporal y observar la morfología. La señal no se modifica en ningún momento. Lo que queda de esta sesión es que un mismo registro admite varias representaciones y que ninguna basta por sí sola, algo que se comprueba al comparar el trazado temporal con el histograma de amplitudes.

En el **Laboratorio 02** la pregunta es qué frecuencias componen la señal y en qué momento aparecen. Se incorpora el análisis espectral y, con la STFT, el dominio conjunto de tiempo y frecuencia. Aparece también la primera decisión con consecuencias sobre el resultado, el tamaño de ventana, donde ya no basta seguir un procedimiento fijo. Trabajar con tres registros permite contrastar señales limpias con otras que presentan deriva de línea base, diferencia que en el dominio temporal resulta ambigua y en el espectrograma se distingue sin dificultad.

En el **Laboratorio 03** la pregunta es cómo modificar la señal sin degradarla. Es la única sesión que interviene sobre los datos y por eso la única que puede deteriorarlos. Se recurre a una señal sintética porque contar con una referencia limpia permite cuantificar ese deterioro. Aquí no se ajusta un parámetro aislado sino un conjunto: familia, tipo de filtro, frecuencia de corte, orden y tratamiento de fase.

El orden de las sesiones no es arbitrario. Elegir una frecuencia de corte exige haber visto antes el espectro, y leer un espectro exige conocer la señal en el tiempo. El error de diseño analizado en la última sesión, un corte demasiado bajo que aplana el complejo QRS, solo se entiende porque en las sesiones previas se determinó dónde se concentra la energía de ese complejo.

---

## 14. Conclusiones

Filtrar una señal biomédica es una cadena de decisiones y no la aplicación mecánica de una función. La secuencia parte de la inspección de la señal, continúa con su caracterización espectral, prosigue con la identificación del ruido y la selección justificada del filtro y sus parámetros, y concluye con la validación de que la información fisiológica se ha preservado.

La Transformada Z proporciona el marco que conecta la ecuación en diferencias, la función de transferencia, la posición de polos y ceros, la estabilidad y la respuesta en frecuencia. La Transformada de Fourier es un caso particular de aquella y aporta el diagnóstico previo a cualquier diseño.

La distinción entre FIR e IIR no establece una jerarquía sino un compromiso entre estabilidad garantizada y linealidad de fase por un lado, y eficiencia computacional por el otro. Un filtro correctamente diseñado atenúa la interferencia sin alterar la morfología de la señal, y verificarlo requiere evidencia cuantitativa además de inspección visual.

---

## Referencias

- Valdez Portocarrero, A. Laboratorio de Filtros FIR, IIR y Transformada Z. Introducción a las Señales Biomédicas, UPCH.
- PhysioNet. Bases de datos MIT-BIH Arrhythmia y Normal Sinus Rhythm.
- Documentación oficial de SciPy Signal Processing.
- Documentación de WFDB Python y NeuroKit2.