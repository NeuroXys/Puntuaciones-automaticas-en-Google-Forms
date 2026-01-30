Este script de Google Apps Script recorre todas las preguntas de un formulario de Google configurado como cuestionario y asigna automáticamente 1 punto a cada pregunta.

Está pensado para ahorrar tiempo cuando se trabaja con formularios largos o cuando se necesita aplicar una puntuación uniforme sin editar cada pregunta manualmente desde la interfaz de Google Forms.

# Qué hace el script:

* Accede al formulario activo.

* Detecta las preguntas compatibles con puntuación.

* Establece el valor de cada pregunta en 1 punto.

* Mantiene intactas las respuestas y el resto de la configuración del formulario.

* Si el documento no era un formulario, lo convierte.

# Casos de uso habituales:

* Exámenes tipo test rápidos.

* Formularios educativos con corrección automática.

* Ajustes masivos de puntuación tras duplicar un formulario.

## Código

<details>
<summary>Click aquí para ver el código</summary>

```ruby
function asignarUnPuntoATodas() {
  // 👉 Sustituye este ID por el de tu formulario (la parte entre /d/ y /edit)
  var form = FormApp.openById("ID-de-tu-formulario");

  // Condicional que convierte el documento automáticamente en un formulario, en caso de que no lo fuera.
  if (!form.isQuiz()) {
    form.setIsQuiz(true);
    Logger.log("El formulario no era un cuestionario. Se ha convertido en uno automáticamente.");
  }

  var items = form.getItems();
  var modificadas = 0;
  var omitidas = 0;

  items.forEach(function(item) {
    try {
      switch (item.getType()) {
        case FormApp.ItemType.MULTIPLE_CHOICE:
          item.asMultipleChoiceItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.CHECKBOX:
          item.asCheckboxItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.LIST:
          item.asListItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.TEXT:
          item.asTextItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.PARAGRAPH_TEXT:
          item.asParagraphTextItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.SCALE:
          item.asScaleItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.GRID:
          item.asGridItem().setPoints(1);
          modificadas++;
          break;
        case FormApp.ItemType.CHECKBOX_GRID:
          item.asCheckboxGridItem().setPoints(1);
          modificadas++;
          break;
        default:
          omitidas++;
      }
    } catch (e) {
      omitidas++;
      console.warn("Omitido tipo " + item.getType() + ": " + e);
    }
  });

  Logger.log("Preguntas actualizadas con 1 punto: " + modificadas);
  Logger.log("Elementos omitidos: " + omitidas);
}
```

</details>

**Solo es necesario pegar el script en el editor de Apps Script del formulario, colocar la ID del formulario en "ID-de-tu-formulario" y ejecutarlo una vez.**

Shield: [![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
