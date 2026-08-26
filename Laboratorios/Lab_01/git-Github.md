# Laboratorio 01 - Git y GitHub

**Curso:** Introducción a Señales Biomédicas
**UPCH - 2026-II | Grupo 4**

Resumen de los contenidos de la sesión teórica y del laboratorio 01.

---

## 1. Git y GitHub

Git es un sistema de control de versiones distribuido que se ejecuta localmente y registra el historial de cambios de un proyecto: qué línea se modificó, cuándo y por quién. No requiere conexión a internet.

GitHub es una plataforma que aloja repositorios Git en la nube y añade funciones de colaboración: control de colaboradores, issues y pull requests.

## 2. Instalación y configuración

Instalación según sistema operativo:

- Windows: instalador de git-scm.com
- Linux: `sudo apt-get install git`
- macOS: `brew install git`

Verificación y configuración de identidad:

```bash
git --version
git config --global user.name "Nombre Apellido"
git config --global user.email "correo@upch.pe"
```

El correo configurado debe coincidir con el de la cuenta de GitHub para que los commits se asocien correctamente al perfil del autor.

## 3. Áreas de trabajo de Git

Git organiza el flujo de cambios en cuatro áreas:

```
Working Directory --git add--> Staging Area --git commit--> Local Repository --git push--> Remote Repository
```

| Área | Función |
|---|---|
| Working Directory | Carpeta de trabajo donde se editan los archivos |
| Staging Area | Zona intermedia donde se seleccionan los cambios que formarán el próximo commit |
| Local Repository | Historial de commits almacenado localmente |
| Remote Repository | Copia compartida alojada en GitHub |

En sentido inverso, `git pull` incorpora los cambios del remoto al repositorio local y `git clone` descarga un repositorio completo por primera vez.

Cabe precisar que `git commit` no publica los cambios: estos permanecen en el repositorio local hasta ejecutar `git push`.

## 4. Flujo de trabajo básico

```bash
git status                  # estado de los archivos
git add .                   # mover cambios al staging
git commit -m "mensaje"     # registrar el commit
git push                    # publicar en el remoto
git pull                    # incorporar cambios del remoto
```

Se recomienda ejecutar `git pull` al inicio de cada sesión de trabajo y no al final, para reducir la probabilidad de conflictos.

Revisión del historial:

```bash
git log --oneline
git log --oneline --graph --decorate --all
```

Cada commit se identifica mediante un hash SHA que permite referenciar versiones específicas.

## 5. Ramas

Una rama constituye una línea de desarrollo independiente. La rama principal (`main`) debe mantenerse en un estado funcional y estable.

```bash
git branch                  # listar ramas locales
git checkout -b mi_rama     # crear rama y cambiar a ella
git checkout main           # retornar a la rama principal
git merge mi_rama           # integrar los cambios
git push -u origin mi_rama  # publicar la rama en el remoto
git branch -d mi_rama       # eliminar rama ya integrada
```

En trabajo colaborativo, el uso de ramas permite el desarrollo paralelo sin interferencia entre los integrantes.

## 6. Conflictos de fusión

Un conflicto se produce cuando dos ramas modifican las mismas líneas de un mismo archivo. Git no puede resolver la ambigüedad de forma automática y delega la decisión al usuario, delimitando ambas versiones:

```
<<<<<<< HEAD
versión de la rama actual
=======
versión de la rama entrante
>>>>>>> mi_rama
```

Procedimiento de resolución:

1. Editar el archivo conservando la versión correcta y eliminando los marcadores.
2. `git add <archivo>`
3. `git commit` y `git push`

## 7. Integración con VS Code

VS Code permite ejecutar el flujo completo desde la interfaz gráfica mediante el panel Source Control. La autenticación se realiza con GitHub CLI (`gh auth login`).

Funciones principales: Initialize Repository, Commit, Publish Branch, cambio de rama desde la barra de estado y resolución de conflictos mediante las opciones Accept Current / Accept Incoming / Accept Both.

Configuración útil: asignar el valor `push` al parámetro **Git: Post Commit Command** en Settings, de modo que cada commit se publique automáticamente.

## 8. Markdown

Sintaxis empleada en los archivos de documentación del repositorio:

| Sintaxis | Resultado |
|---|---|
| `# Texto` | Encabezado de nivel 1 |
| `## Texto` | Encabezado de nivel 2 |
| `- Texto` | Viñeta |
| `**Texto**` | Negrita |
| `*Texto*` | Cursiva |
| `` `Texto` `` | Código en línea |
| `[Texto](url)` | Enlace |
| `![alt](ruta.png)` | Imagen |

Vista previa en VS Code: `Ctrl+Shift+V`.

## 9. Buenas prácticas

- Commits de alcance reducido y con mensajes descriptivos del cambio realizado.
- Nomenclatura de ramas explícita: `feature/adquisicion-emg`, `exp/semana4`.
- Gestión de archivos de gran tamaño mediante Git LFS.
- Integración a `main` a través de pull requests en lugar de push directo.

## 10. Infografía

![Infografía Git y GitHub](infografia-git.png)

---

# Sesión teórica: fundamentos de señales biomédicas

## 11. Origen de las señales biomédicas

El organismo mantiene su equilibrio interno mediante la **homeostasis**, regulada de forma conjunta por el sistema nervioso autónomo y el sistema endocrino. Ningún sistema fisiológico opera de manera aislada.

**Retroalimentación negativa:** un cambio en una variable fisiológica activa la secuencia sensor → centro de control → efector, que genera un efecto opuesto al estímulo inicial y restaura el valor de referencia. Ejemplo: la termorregulación.

**Retroalimentación positiva:** el cambio inicial desencadena respuestas que lo amplifican, generando una secuencia creciente que se mantiene hasta la desaparición del estímulo o hasta un evento terminal. Ejemplo: el trabajo de parto.

A escala celular, este balance dinámico requiere la regulación continua de concentraciones y gradientes iónicos, mediada por proteínas transmembrana. El proceso implica un costo energético cubierto por la oxidación mitocondrial (síntesis de ATP).

**Mecanismos de transporte:**

| Tipo | Mecanismo | Consumo de ATP |
|---|---|---|
| Pasivo | Difusión simple y difusión facilitada, a favor del gradiente | No |
| Activo | Transporte contra gradiente | Sí |

## 12. Potencial de membrana

Existe una diferencia de cargas iónicas entre los compartimientos intracelular y extracelular, siendo el interior electronegativo respecto al exterior.

Los iones determinantes son K⁺, Na⁺, Cl⁻ y un anión orgánico (A⁻). El K⁺ y el A⁻ presentan mayor concentración intracelular, mientras que el Na⁺ y el Cl⁻ predominan en el medio extracelular.

Todas las células se encuentran polarizadas, pero solo las células excitables especializadas son capaces de generar variaciones de voltaje.

**Potencial de difusión.** Diferencia de voltaje transmembrana generada por el desplazamiento de un ion a favor de su gradiente de concentración. Cada ion está sometido a dos fuerzas:

- **Fuerza química:** gradiente de concentración.
- **Fuerza eléctrica:** fuerza electrostática dependiente de la carga del ion y del potencial del compartimiento.

Ambas fuerzas se encuentran desbalanceadas; el K⁺ presenta una concentración intracelular aproximadamente 40 veces mayor que la extracelular.

**Conductancia.** Los canales iónicos actúan como conductores de la corriente transmembrana. La conductancia (g) se expresa en siemens (S) y es constante para un canal iónico determinado.

**Potencial de membrana en reposo.** Depende de tres factores: el potencial de difusión del sodio, el potencial de difusión del potasio y la bomba sodio-potasio (ATPasa). En reposo, la permeabilidad de la membrana al K⁺ es considerablemente mayor que al Na⁺. Su estimación se realiza mediante la **ecuación de Goldman**, que incorpora la permeabilidad diferencial de la membrana a cada especie iónica.

**Bomba Na⁺–K⁺.** Proteína transmembrana que en cada ciclo introduce 2 K⁺ y expulsa 3 Na⁺, extrayendo una carga positiva neta del interior celular. Es responsable del mantenimiento del potencial de membrana.

## 13. Clasificación de las señales biomédicas

Los procesos fisiológicos generan señales de muy bajo potencial eléctrico que reflejan la actividad de cada sistema. Se clasifican en:

- **Bioquímicas:** hormonas, neurotransmisores.
- **Eléctricas:** potenciales y corrientes.
- **Mecánicas:** presión, temperatura.

Las **señales bioeléctricas** se adquieren mediante electrodos que registran las variaciones de potencial generadas por la actividad fisiológica. Su principal limitación es la presencia de **ruido**, que degrada la calidad del registro.

Principales señales bioeléctricas:

| Sigla | Señal | Origen |
|---|---|---|
| — | Potencial de acción | Señal base de la que derivan las demás |
| EMG | Electromiograma | Actividad eléctrica de células musculares |
| ECG | Electrocardiograma | Actividad eléctrica cardíaca |
| EEG | Electroencefalograma | Actividad eléctrica cerebral |
| EGG | Electrogastrograma | Actividad contráctil del estómago |
| PCG | Fonocardiograma | Actividad mecánica del corazón (registro acústico) |
| PC | Pulso carotídeo | Presión de la arteria carótida |
| ERG | Electrorretinograma | Respuesta eléctrica de la retina |
| EOG | Electrooculograma | Actividad de la musculatura ocular |

El registro de estas señales a lo largo del tiempo constituye una serie temporal unidimensional. Su comparación con rangos de referencia permite detectar alteraciones: las patologías cardíacas se manifiestan en el ECG y los trastornos neurológicos como la epilepsia, en el EEG.

## 14. Artículos analizados en clase

**Velocidad de conducción nerviosa en neuropatía periférica diabética.** Metaanálisis de 26 estudios. En pacientes con diabetes tipo 2 se reporta una velocidad de conducción promedio de 42.12 m/s y una amplitud de 4.68 µV, ambas por debajo de los rangos de referencia. El nervio sural resulta adecuado para esta evaluación por ser puramente sensorial, superficial y accesible de forma no invasiva, lo que permite la detección en etapa subclínica y la prevención de complicaciones como úlceras de pie diabético y amputaciones.

**Integración de biosensores en la predicción de estados emocionales.** Estudio experimental de medidas repetidas con 39 participantes, orientado a determinar si la incorporación de biosensores periféricos mejora la precisión de la clasificación de emociones evocadas por estímulos musicales. Empleando clasificadores KNN, la banda beta2 registrada en la región prefrontal mediante EEG alcanzó una precisión de 79.1 % de forma aislada, mientras que la adición de ECG, presión arterial, EMG, EOG, respiración, EGG y velocimetría de bioimpedancia no produjo mejoras significativas. Se concluye que estos sensores capturan variaciones fisiológicas reales pero redundantes respecto a la información ya contenida en el EEG frontal.

---

## Referencias

- Meza, M.; De La Cruz, L. *Guía N° 1 - Organización de repositorio en GitHub*, versión 1.2, 2026.
- Meza, M. *Getting Started with Git and GitHub: From Zero to Teamwork.* Medium.
- Meza, M. *VSCODE and Markdown: A Perfect Combination.* Medium.
- Prashant, P.; Pal, S.; Bansal, A.; Fotedar, S. *Nerve conduction velocity studies in diabetic peripheral neuropathy involving sural nerve - A meta-analysis.* Journal of Family Medicine and Primary Care, 2024;13(10):4469-4475.
- Pan, T.F.; Nandi, B.; Campusano, R.; Simon, A.J.; Ziegler, D.A.; Gazzaley, A.; et al. *When more is not better: Predicting human emotional states with brain and bodily biosensors.* Biomedical Signal Processing and Control, 2025;113:109038.