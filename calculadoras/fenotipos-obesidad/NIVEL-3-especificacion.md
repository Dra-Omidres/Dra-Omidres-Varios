# Nivel 3 — Propuesta inicial de alimentación y ejercicio

Especificación funcional del producto de pago que continúa el embudo.
Documento de diseño: describe **qué** debe hacer el sistema y **qué no**, para que
quien lo implemente no tenga que inferirlo.

> **Estado:** borrador para revisión de la Dra. Omidres. No implementado.

---

## 1. Principio rector

**El automatismo entrega arquitectura, no prescripción.**

Esta frase resuelve la mayor parte de las decisiones de diseño. Un plan que dice
*"palma de proteína, puño de vegetales, mano ahuecada de carbohidrato"* es material
educativo. Un plan que dice *"120 g de pollo, 45 g de arroz"* es una prescripción
dietética individualizada, competencia reservada de nutricionista-dietista o médico
en prácticamente todas las jurisdicciones de la región.

La diferencia no es cosmética: determina si el entregable automático es defendible.
Y para quien está empezando, el método del plato funciona igual o mejor, porque no
se abandona a la tercera pesada.

Lo mismo en ejercicio: se entrega **progresión estructurada** (frecuencia, tipo,
esfuerzo percibido, cómo avanzar), no dosis prescrita.

---

## 2. El embudo completo

| Nivel | Entregable | Precio | Quién valida |
|---|---|---|---|
| 1 | Calculadora de IMC y cintura (ADA 2026) | Gratis | Automático |
| 2 | Calculadora de fenotipos | Gratis | Automático |
| 3 | **Propuesta inicial de alimentación y/o ejercicio** | **De pago** | Automático, con plantillas cerradas |
| 4 | Plan individualizado con gramajes e intercambios | De pago | **Revisión humana obligatoria** |

El nivel 3 es modular: el paciente elige **alimentación**, **ejercicio** o **ambas**.

**La línea profesional termina en el nivel 2 y es gratuita.** No se le cobra al
profesional y no se le ofrece el nivel 3: es canal de distribución y derivación,
no cliente.

---

## 3. La decisión arquitectónica más importante

### El cribado va ANTES del cobro. Siempre.

Si el pago ocurre primero, el sistema acaba cobrando a una paciente con insulina,
o a alguien con cribado positivo de trastorno alimentario, y entonces hay que
devolver el dinero después de haberle dicho que no puede usar lo que compró.
Eso es una mala experiencia y un problema reputacional evitable.

Orden correcto:

```
Fenotipos (nivel 2)
   ↓
Formulario de admisión  ← aquí ocurre el cribado
   ↓
¿Hay criterio de exclusión?
   ├── SÍ  → No se ofrece compra. Se deriva a consulta.
   └── NO  → Se muestra el precio y se cobra
                ↓
           Entrega de la propuesta
```

---

## 4. Formulario de admisión

### Bloque A — Identificación y consentimiento
Nombre, edad, correo, país. Consentimiento explícito e informado para tratar datos
de salud (ver §8). Casilla no premarcada.

### Bloque B — Cribado de exclusión *(bloqueante)*
Reutiliza literalmente la lógica ya implementada en `calculadora.html`
(funciones `bloqueos()` y `MOTIVOS`). No reescribir: importar el mismo criterio
para que ambos productos nunca diverjan.

### Bloque C — Historia clínica resumida
Diagnósticos actuales · medicación completa con dosis · cirugías previas ·
peso máximo y mínimo histórico · tratamientos de peso previos y qué pasó ·
analítica reciente si la tiene (HbA1c, perfil lipídico, TSH, función renal y hepática).

### Bloque D — Diario de alimentación de 72 horas
Tres días, **incluyendo al menos un día de fin de semana**. Registro de hora,
alimento, cantidad aproximada en medidas caseras, lugar y compañía, y sensación de
hambre previa (0–10).

> **Advertencia metodológica que debe constar en el material.** El infrarreporte
> en registros dietéticos autoadministrados es un sesgo bien documentado y es mayor
> en personas con obesidad. *No se incluye aquí una cifra concreta porque no se ha
> verificado una fuente para ella.* El diario sirve para identificar **patrones**
> (horarios, estructura, contexto emocional, tipo de alimento), no para calcular
> calorías con precisión. El sistema no debe presentar un total calórico del diario
> como si fuera exacto.

### Bloque E — Alergias e intolerancias
Alergias alimentarias diagnosticadas · intolerancias · enfermedad celíaca ·
aversiones fuertes · restricciones religiosas o éticas (vegetariana, vegana, halal,
kosher, vigilia y abstinencia).

### Bloque F — Preferencias y contexto
Quién cocina · presupuesto · acceso a alimentos · tiempo disponible · horarios de
trabajo y turnos · comidas fuera de casa por semana · horas de sueño.

### Bloque G — PAR-Q+ *(solo si eligió ejercicio)*
Cuestionario de aptitud para la actividad física. Es el instrumento estándar,
publicado por la Canadian Society for Exercise Physiology.

> **Pendiente antes de implementar:** verificar sus condiciones de uso y licencia.
> No se ha confirmado en este documento que su reproducción sea libre.

---

## 5. Criterios de exclusión duros

Bloquean la venta del módulo correspondiente y derivan a consulta.

| Criterio | Bloquea | Por qué |
|---|---|---|
| Insulina o sulfonilureas | Alimentación | Déficit calórico sin ajuste de dosis → hipoglucemia |
| Enfermedad renal crónica | Alimentación | Requiere ajustar proteínas, potasio y fósforo |
| Embarazo o lactancia | Alimentación | Requerimientos propios; no aplica déficit |
| Warfarina o acenocumarol | Alimentación | La vitamina K de la dieta altera el INR |
| Cirugía bariátrica previa | Alimentación | Pauta y suplementación específicas |
| Cribado positivo de TCA | Alimentación | Un plan restrictivo puede ser iatrogénico |
| Menor de 18 años | Ambos | Marco descrito y validado en adultos |
| Evento cardiovascular < 6 meses, o angina de esfuerzo | Ejercicio | Requiere valoración y prueba de esfuerzo |
| Retinopatía diabética proliferativa | Ejercicio | Contraindica esfuerzo intenso y maniobra de Valsalva |
| Lesión o úlcera activa en el pie | Ejercicio | Riesgo de progresión con carga |
| Hipertensión no controlada | Ejercicio | Requiere control previo |
| Artrosis severa o movilidad muy limitada | Ejercicio | Necesita adaptación individual |
| Neuropatía autonómica | Ejercicio | Respuesta cardiovascular alterada al esfuerzo |

### Nota sobre el cribado de trastorno alimentario

Es el más importante y el que más fácilmente se omite. El fenotipo **hambre
emocional se solapa clínicamente con el trastorno por atracón**, de modo que el
sistema va a identificar ese perfil con frecuencia. La ADA 2026 pide cribado
formal (QEWP-5) antes de intervenir.

Debe ser **bloqueo**, no pregunta informativa. Y el mensaje de derivación debe
cuidar el tono: no es un rechazo, es una condición tratable que necesita otro
punto de entrada.

---

## 6. Estructura del entregable

### 6.1 Alimentación

1. **Resumen de tu perfil** — fenotipo(s), patrones detectados en el diario de 72 h.
2. **Arquitectura del plato** — método de mano, con ilustración.
3. **Estructura de comidas y horarios** — varía según fenotipo (§7).
4. **Lista de compras por categorías** — adaptada a presupuesto y acceso.
5. **Tres ejemplos de día tipo** — sin gramajes, en medidas caseras.
6. **Estrategias conductuales del fenotipo** — las ya redactadas en `calculadora.html`, ampliadas.
7. **Señales de alarma** — cuándo detenerse y consultar.
8. **Qué llevar a la consulta** — resumen imprimible para la primera visita.

### 6.2 Ejercicio

1. **Resultado del PAR-Q+** y qué implica.
2. **Punto de partida** según nivel de actividad actual declarado.
3. **Progresión de 8 semanas** — frecuencia, tipo, duración y esfuerzo percibido (escala RPE). Sin dosis prescrita.
4. **Fuerza 2–3 veces por semana** — prioritario para preservar masa muscular durante la pérdida de peso, y especialmente en el perfil de quemador lento.
5. **Movimiento no deportivo (NEAT)** — caminar, escaleras, romper el sedentarismo cada hora.
6. **Señales para detenerse** — dolor torácico, disnea desproporcionada, mareo, palpitaciones.

---

## 7. Diferenciación por fenotipo

Estrategias **nutricionales y conductuales**. No incluye fármacos: el nivel 3 no
prescribe ni sugiere medicación.

| Fenotipo | Eje de la propuesta |
|---|---|
| **Cerebro hambriento** | Volumen y densidad calórica: empezar por verduras y proteína, alto volumen con baja densidad, platos más pequeños, sin fuente en la mesa, comer despacio y sin pantallas |
| **Intestino hambriento** | Duración de la saciedad: proteína suficiente en cada comida, fibra y grasa saludable, tres comidas con horarios estables en lugar de picoteo continuo, sin bebidas azucaradas |
| **Hambre emocional** | Regulación: distinguir hambre física de emocional, registro de antecedente emocional, alternativa de 10 minutos, y **derivación a apoyo psicológico como parte del plan, no como añadido** |
| **Quemador lento** | Masa muscular y gasto: fuerza 2–3 veces por semana, proteína suficiente, aumento del NEAT, sueño de 7 horas o más |

**Perfil no concluyente o múltiple:** se entrega la base común más el rasgo de
mayor puntaje, declarando que el perfil no fue concluyente. No forzar una etiqueta.

> Base: Patti et al., *Medicina* 2025, plantea manejo nutricional diferenciado por
> fenotipo. Existe además una prueba de concepto de intervención de estilo de vida
> adaptada al fenotipo (*eClinicalMedicine* 2023), **de centro único y no
> aleatorizada** — no permite afirmar superioridad. No se debe comunicar como
> evidencia establecida.

---

## 8. Generación: plantillas cerradas, no LLM libre

**Un modelo de lenguaje en modo libre no debe redactar el plan.** Alucina gramajes,
intercambios e interacciones, y aquí eso tiene consecuencias clínicas.

Arquitectura correcta:

```
(fenotipo, restricciones, preferencias, presupuesto)
        ↓  tabla de decisión determinística
   plantilla precargada, escrita y revisada por la Dra. Omidres
        ↓  relleno de variables acotadas
              entregable
```

Todo texto clínico sale de una biblioteca fija, revisada una vez y versionada.
El sistema **selecciona y combina**; no redacta. Si una combinación no tiene
plantilla, el caso va a revisión humana en lugar de improvisar.

---

## 9. Protección de datos

En el momento en que se recoge historia clínica, alergias y diario alimentario,
se tratan **datos de salud** — categoría especial. La calculadora actual no guarda
nada; el nivel 3 sí.

Requisitos mínimos:

- Base legal y **consentimiento explícito**, separado de los términos generales.
- Cifrado en tránsito y en reposo.
- Política de retención y procedimiento de borrado a solicitud.
- Contratos de encargado de tratamiento con cada proveedor de la cadena
  (formularios, automatización, correo, pasarela de pago).
- **Con una sola usuaria en la Unión Europea, aplica el RGPD.** Conviene decidir
  desde el inicio si se acepta tráfico europeo o se restringe.
- Auditar la cadena Google Forms → Zapier → Mailjet **antes** de lanzar, no después.

---

## 10. Lo que nunca se automatiza

- Gramajes, intercambios y cálculo calórico individualizado.
- Cualquier mención, sugerencia o ajuste de medicación.
- Planes por debajo de 1 200 kcal/día.
- Suplementos, más allá de recomendar consultarlos.
- Casos con cualquier criterio de exclusión de §5.
- Interpretación de análisis de laboratorio.

---

## 11. Posicionamiento comercial

**No vender "tu fenotipo de obesidad".** Vender **"tu plan inicial personalizado"**,
donde el fenotipo es un insumo más junto al diario de 72 h, la historia y las
preferencias.

Tres razones:

1. **Científica.** La clasificación por cuestionario no está validada (Gołacki 2026:
   50,3 % sin fenotipo asignable; Koufakis 2025: *"limited validation, uncertain
   predictive value"*).
2. **Competitiva.** Phenomix Sciences lanzó en junio de 2026 la venta directa al
   paciente de MyPhenome®, con test genético de laboratorio. Competir en "te
   identificamos tu fenotipo" contra eso, con un cuestionario, es una pelea perdida.
3. **De marca.** Colgar la promesa comercial de un constructo que la literatura
   llama heurístico es el mayor riesgo reputacional del proyecto.

### Decisión pendiente: acompañamiento humano

El referente de comparación es **Second Nature** (Reino Unido), que hace exactamente
este embudo —quiz gratuito basado en Acosta 2021 → programa de pago— e incluye
**acceso diario a dietista colegiada**, desde ~£33–40/mes (cifras de sitios de
reseñas terceros; verificar).

Hay que elegir explícitamente y comunicarlo sin ambigüedad:

- **Opción A — automático y económico.** Entrega inmediata, precio bajo, sin humano.
  Se compite en acceso y precio, no en acompañamiento.
- **Opción B — con validación profesional.** Revisión antes de entregar. Más caro,
  defendible, y convierte el producto en un servicio profesional real.

Donde se arruinan las reseñas es al prometer acompañamiento profesional y entregar
un PDF automático. **Cualquiera de las dos funciona; la mezcla ambigua, no.**

---

## 12. Referencias

Las mismas de `calculadora.html`, sección Referencias. Todas verificadas en bases
indexadas. Los datos de volumen y páginas se incluyen solo donde pudieron
confirmarse.

**Pendiente de verificación:** la asignación fármaco-fenotipo concreta del brazo
guiado de Acosta 2021 no se pudo contrastar contra el texto completo. No se
reproduce en ningún material hasta confirmarla. Irrelevante para el nivel 3, que
no trata medicación, pero sí para el modo profesional del nivel 2.

---

## Anexo — Migración a un instrumento validado (pendiente)

El cuestionario actual de la calculadora de fenotipos **no está validado**. Existe una
alternativa mejor y está en español.

### EFCA — Escala de Fenotipos de Comportamiento Alimentario

**Cita:** Anger VE, Formoso J, Katz MT. Escala de Fenotipos de Comportamiento
Alimentario (EFCA), análisis factorial confirmatorio y propiedades psicométricas.
*Nutr Hosp.* 2022;39(2):405. doi:10.20960/nh.03849 · Acceso abierto.
Escala original: *Actualización en Nutrición.* 2020;21(3):73-79.

**Lo verificado:**

| Aspecto | Dato |
|---|---|
| Estructura | 16 ítems, cinco subescalas |
| Subescalas | Hedónico · compulsivo · emocional · desorganizado · hiperfágico |
| Respuesta | Likert de 1 (nunca) a 5 (siempre) |
| Particularidad | El ítem 9 puntúa de forma **inversa** |
| Muestra de validación | 300 adultos |
| Consistencia interna | α > 0,70 en escala total y subescalas |
| Análisis factorial confirmatorio | CFI 0,97 · TLI 0,96 · RMSEA 0,05 · SRMR 0,04 |
| Validez concurrente | Correlación positiva y significativa con IMC |
| Validaciones posteriores | Portugués de Brasil (*Arch Endocrinol Metab* 2025, α=0,83); uso en México (*Horizonte Sanitario* 2025, α=0,86) |
| Uso con fármacos | *Nutrients* 2026, cohorte de São Paulo, perfiles de respuesta por fármaco antiobesidad |

**Lo que falta y por qué:** el texto de los 16 ítems, el método de puntuación
detallado y los puntos de corte de clasificación. No se pudieron obtener porque el
entorno de construcción tiene bloqueado el acceso a SciELO, CONICET, DOI, LILACS y
SciELO México por política de red. **El artículo es de acceso abierto**: se descarga
directamente desde SciELO España o mediante el DOI.

**Advertencia conceptual que no debe perderse.** La EFCA **no** es la versión en
español del modelo de Acosta. Son cinco fenotipos conductuales de desarrollo
independiente en Argentina. Solo el componente emocional es comparable. Si se migra,
la herramienta deja de hablar de "los cuatro fenotipos de Acosta" y pasa a hablar de
"los cinco fenotipos conductuales de la EFCA". Es un cambio de marco, no una
traducción.

### Alternativa complementaria — NZ-EBQ

Cubre tres de los cuatro fenotipos de Acosta (saciación, saciedad posprandial,
alimentación emocional) y **no mide quemador lento**. Validado en n=977
(*Appetite* 2023) y revalidado en *Nutrients* 2025 con solo 12,3–13,0 % de casos no
clasificables, muy por debajo del 50,3 % de Gołacki. En inglés: requeriría traducción
y validación propia, que es un proyecto de investigación, no una tarea de desarrollo.

### Tres opciones

| Opción | Marco | Ventaja | Coste |
|---|---|---|---|
| **A** · Mantener el cuestionario actual | Acosta, 4 fenotipos | Ya funciona; coherente con la literatura que la Dra. divulga | Sin validar. Declarado en pantalla |
| **B** · Migrar a EFCA | EFCA, 5 fenotipos | Instrumento validado, en español, desarrollado en la región | Cambia el marco. Requiere los ítems y verificar condiciones de uso |
| **C** · Híbrido | Acosta para explicar, EFCA para medir | Lo mejor de ambos | Riesgo de confundir dos taxonomías. **Solo si se explicita la diferencia** |

**Requisitos previos a cualquier migración:** obtener el artículo original, verificar
las condiciones de uso de la escala y confirmar si los autores exigen permiso para
su reproducción en una herramienta digital de acceso público.
