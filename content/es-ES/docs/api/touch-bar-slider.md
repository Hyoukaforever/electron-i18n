## Clase: TouchBarSlider

> Crea un control deslizante en la barra táctil para aplicaciones nativas de macOS

Proceso: [Main](../tutorial/quick-start.md#main-process)

### `new TouchBarSlider(options)` *Experimental*

* `opciones` Objeto 
  * `label` Cadena (opcional) - Texto de etiqueta.
  * `value` Entero (opcional) - Valor seleccionado.
  * `minValue` Entero (opcional) - Valor mínimo.
  * `maxValue` Entero (opcional) - Valor máximo.
  * `change` Función (opcional) - Función para llamar cuando se cambie el control deslizante. 
    * `newValue` Número - El valor que el usuario seleccionó en el control deslizante

### Propiedades de Instancia

Las siguientes propiedades está disponibles en instancias de `TouchBarSlider`:

#### `touchBarSlider.label`

Un `String` que representa el texto actual del control deslizante. Cambiar este valor actualiza inmediatamente el control deslizante en la barra táctil.

#### `touchBarSlider.value`

Un `Number` que representa el valor actual del control deslizante. Cambiar este valor actualiza inmediatamente el control deslizante en la barra táctil.

#### `touchBarSlider.minValue`

Un `Number` que representa el valor mínimo actual del control deslizante. Cambiar este valor actualiza inmediatamente el control deslizante en la barra táctil.

#### `touchBarSlider.maxValue`

Un `Number` que representa el valor máximo actual del control deslizante. Cambiar este valor actualiza inmediatamente el control deslizante en la barra táctil.