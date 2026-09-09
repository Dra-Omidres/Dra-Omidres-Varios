# Materiales docentes — Dra. Omidres Pérez de Carvelli

Endocrinología · Medicina Interna · Salud Digital

## Contenido

```
index.html                        Portal de entrada (abre este archivo)
cursos/
  └── ia-en-salud/
      └── taller-llm-comunicacion/
          ├── opcion-a-tiroides/     taller.html + PDF   (nódulo tiroideo ACR TI-RADS 5)
          └── opcion-b-menopausia/   taller.html + PDF   (osteoporosis posmenopáusica)
```

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
