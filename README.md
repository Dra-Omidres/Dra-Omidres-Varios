# Materiales docentes — Dra. Omidres Pérez de Carvelli

Endocrinología · Medicina Interna · Salud Digital

## Contenido

```
index.html                        Portal de entrada (abre este archivo)
calculadoras/
  └── fenotipos-obesidad/
      ├── calculadora.html         Fenotipos de obesidad (modo paciente / modo profesional)
      └── NIVEL-3-especificacion.md  Diseno del producto de pago (borrador, sin implementar)
cursos/
  └── ia-en-salud/
      └── taller-llm-comunicacion/
          ├── opcion-a-tiroides/     taller.html + PDF   (nódulo tiroideo ACR TI-RADS 5)
          └── opcion-b-menopausia/   taller.html + PDF   (osteoporosis posmenopáusica)
```

## Calculadora de fenotipos de obesidad

Estima a cuál de los cuatro perfiles descritos por Acosta et al. (*Obesity* 2021) se
aproxima más el patrón del paciente, a partir de un cuestionario de 16 ítems.

**Antes de usarla, lee esto.** El fenotipado de referencia requiere comida *ad libitum*,
gammagrafía de vaciamiento gástrico, calorimetría indirecta y escala HADS. **Un cuestionario
no reproduce esos resultados**: en el único estudio publicado que lo intentó (Gołacki et al.,
*Endokrynol Pol* 2026), el 50,3 % de los pacientes quedó sin fenotipo asignable, y los autores
concluyen que la herramienta es exploratoria y no apta para guiar tratamiento. La calculadora
lo declara de forma explícita en pantalla. **Es material educativo y de preparación de consulta,
no un instrumento diagnóstico.**

Características:

- **Dos instrumentos seleccionables.** *Modelo de Acosta*: los cuatro fenotipos
  fisiopatológicos, con cuestionario propio sin validar. *Escala EFCA*: instrumento
  validado en español (Anger VE, Formoso J, Katz MT. *Nutr Hosp.* 2022;39(2):405-410),
  16 ítems y cinco subescalas, reproducido bajo licencia CC BY-NC-SA 4.0 con la
  atribución y la declaración de financiación exigidas. **Son taxonomías distintas y
  la herramienta lo advierte de forma explícita.**
- **Dos modos.** *Paciente*: lenguaje sencillo, sin fármacos ni dosis. *Profesional*: métodos
  de referencia, campos para mediciones objetivas (GER, T½ de vaciamiento, HADS) y racional
  farmacológico con sus límites declarados.
- **Confirma primero la adiposidad** con los criterios ADA 2026 (IMC, cintura, cintura/talla,
  umbrales para origen asiático), igual que la calculadora de IMC.
- **Admite perfiles múltiples y el resultado "no concluyente"**, que es un desenlace frecuente
  y esperable, no un fallo.
- **Bloqueos de seguridad.** Señala cuándo el paciente *no* debe iniciar cambios por su cuenta:
  insulina o sulfonilureas, enfermedad renal, embarazo, anticoagulación, cirugía bariátrica
  previa, señales de trastorno de conducta alimentaria, y —para ejercicio— evento cardiovascular
  reciente, retinopatía proliferativa, lesión activa en el pie, hipertensión no controlada o
  movilidad muy limitada.
- **Deriva a valoración** ante señales de hipotiroidismo, ingesta nocturna o riesgo en salud mental.

Antes de publicarla, edita las dos constantes del bloque `CONFIGURACIÓN` al inicio del
`<script>`: `LINK_AGENDA` (enlace de agendamiento) y `LINK_IMC` (calculadora de IMC y cintura).

### Nivel 3 — propuesta de alimentación y ejercicio (en diseño)

`calculadoras/fenotipos-obesidad/NIVEL-3-especificacion.md` documenta el producto de
pago que continuaría el embudo: qué debe hacer, qué no debe automatizarse nunca, los
criterios de exclusión que bloquean la venta **antes** del cobro, y las decisiones
comerciales pendientes. Es un borrador para revisión; no está implementado.

Los archivos `taller.html` son **autocontenidos**: no dependen de internet, de hojas
de estilo externas ni de imágenes. Se abren en cualquier navegador y se imprimen en A4.

---

## 1. Ver o presentar el material

**Desde cualquier computadora o teléfono, sin instalar nada:**

Si GitHub Pages está activado, el material queda publicado en una dirección web fija
que se abre como cualquier página. Para activarlo (una sola vez):

1. En el repositorio, entra en **Settings** → **Pages**
2. En *Source*, elige **Deploy from a branch**
3. Selecciona la rama por defecto y la carpeta **/ (root)** → **Save**
4. GitHub mostrará la URL publicada en esa misma pantalla, tras unos minutos

> **Nota:** GitHub Pages es gratuito en repositorios públicos. En repositorios
> privados requiere un plan de pago (GitHub Pro o superior).

**Sin GitHub Pages:** descarga el ZIP (ver punto 3) y abre `index.html`.

---

## 2. Editar el contenido

### Opción A — En el navegador, sin instalar nada *(recomendada)*

Sirve desde cualquier computadora, incluso prestada.

1. Abre el archivo que quieras cambiar en GitHub
2. Pulsa el icono del **lápiz** (*Edit this file*)
3. Haz los cambios
4. Pulsa **Commit changes…**, escribe una descripción breve y confirma

Para editar varios archivos a la vez: estando en el repositorio, pulsa la tecla **`.`**
(punto). Se abre un editor completo dentro del navegador.

### Opción B — GitHub Desktop (aplicación con interfaz gráfica)

Útil si prefieres editar los archivos con tus propios programas.

1. Instala GitHub Desktop desde <https://desktop.github.com>
2. Inicia sesión con tu cuenta de GitHub
3. **File → Clone repository** y elige este repositorio
4. Edita los archivos en tu computadora
5. En GitHub Desktop: escribe el resumen del cambio → **Commit** → **Push origin**

### Opción C — Línea de comandos (requiere Git instalado)

```bash
git clone https://github.com/Dra-Omidres/Dra-Omidres-Varios.git
cd Dra-Omidres-Varios
```

Después de editar:

```bash
git add -A
git commit -m "Descripción del cambio"
git push
```

> GitHub ya **no acepta la contraseña de la cuenta** para subir cambios desde la
> línea de comandos. Necesitarás un *Personal Access Token* o una clave SSH.
> Si esto te resulta engorroso, usa la Opción A o B.

### ⚠️ Regla imprescindible al trabajar desde varias computadoras

**Antes de empezar a editar, siempre actualiza primero:**

- GitHub Desktop: botón **Fetch origin** / **Pull origin**
- Línea de comandos: `git pull`

Si editas sin actualizar, la segunda computadora entra en conflicto con la primera
y hay que resolverlo a mano. Actualizar toma dos segundos y lo evita por completo.

---

## 3. Usar el material sin internet

1. En la página del repositorio, pulsa el botón verde **Code**
2. Elige **Download ZIP**
3. Descomprime y abre `index.html` con doble clic

Todo funciona sin conexión. Para volver a tener los PDF, están dentro de cada
carpeta de opción.

---

## Nota sobre el contenido

Los casos clínicos incluidos están **anonimizados**: no contienen nombres, documentos
de identidad, fechas de estudio, números de historia clínica, instituciones ni
profesionales identificables. Cada taller documenta explícitamente su verificación de PHI.

Material con fines docentes. No sustituye la consulta médica.
