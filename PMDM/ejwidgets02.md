Esta aplicación es una actividad de Android que contiene cinco `CheckBox`, un `Button` y un `TextView`. La funcionalidad principal de la aplicación es manejar la interacción entre los `CheckBox` y el `Button`, y mostrar mensajes en el `TextView` basados en las combinaciones de los `CheckBox`.

### Detalle del código:

1. **Declaración de variables:**
   - Se declaran variables privadas para los `CheckBox`, `Button` y `TextView`.

2. **Método `onCreate`:**
   - Se llama al método `super.onCreate` y se establece el contenido de la vista con `setContentView`.
   - Se inicializan los componentes de la interfaz de usuario utilizando `findViewById`.
   - Se llama al método `configurarOyentes` para configurar los listeners de los `CheckBox`.
   - Se establece un listener para el `Button` que llama al método `comprobarCombinacion` cuando se hace clic en él.

3. **Método `configurarOyentes`:**
   - Se configuran los listeners para cada `CheckBox`:
     - **`checkBox1`:** Si se selecciona, desmarca todos los demás `CheckBox` y añade un mensaje al `TextView`.
     - **`checkBox2`:** Invierte el estado de `checkBox1`, `checkBox4` y `checkBox5` y añade un mensaje al `TextView`.
     - **`checkBox3`:** Si se selecciona, marca `checkBox1` y desmarca `checkBox4`, y añade un mensaje al `TextView`.
     - **`checkBox4`:** Si se selecciona y `checkBox3` no está seleccionado, se desmarca y añade un mensaje al `TextView`.
     - **`checkBox5`:** Si se selecciona, marca `checkBox1`, desmarca `checkBox4`, habilita el `Button` y añade un mensaje al `TextView`.
     - **`checkBox5` (listener adicional):** Habilita o deshabilita el `Button` basado en el estado de `checkBox5`.

4. **Método `comprobarCombinacion`:**
   - Verifica si todos los `CheckBox` están seleccionados.
   - Si todos están seleccionados, muestra un mensaje de "¡Combinación correcta!" en el `TextView`.
   - Si no, añade un mensaje de "Combinación incorrecta. Intenta de nuevo." al `TextView`.

```kotlin
package dam.pmdm.ejwidgets02

import android.os.Bundle
import android.widget.Button
import android.widget.CheckBox
import android.widget.CompoundButton
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var checkBox1: CheckBox
    private lateinit var checkBox2: CheckBox
    private lateinit var checkBox3: CheckBox
    private lateinit var checkBox4: CheckBox
    private lateinit var checkBox5: CheckBox
    private lateinit var checkButton: Button
    private lateinit var resultText: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Inicializar componentes
        checkBox1 = findViewById(R.id.checkBox1)
        checkBox2 = findViewById(R.id.checkBox2)
        checkBox3 = findViewById(R.id.checkBox3)
        checkBox4 = findViewById(R.id.checkBox4)
        checkBox5 = findViewById(R.id.checkBox5)
        checkButton = findViewById(R.id.checkButton)
        resultText = findViewById(R.id.resultText)

        configurarOyentes()

        // Listener para el botón
        checkButton.setOnClickListener {
            comprobarCombinacion()
        }
    }

    private fun configurarOyentes() {
        checkBox1.setOnClickListener {
            if (checkBox1.isChecked) {
                checkBox2.isChecked = false
                checkBox3.isChecked = false
                checkBox4.isChecked = false
                checkBox5.isChecked = false
                resultText.append("\nOpción 1: Desmarco todas las demás.")
            }
        }

        checkBox2.setOnCheckedChangeListener(object : CompoundButton.OnCheckedChangeListener {
            override fun onCheckedChanged(p0: CompoundButton?, checked: Boolean) {
                    checkBox1.isChecked = !checkBox1.isChecked
                    checkBox4.isChecked = !checkBox4.isChecked
                    checkBox5.isChecked = !checkBox5.isChecked
                    resultText.append("\nOpción 2: Invirtió el estado de 1, 4 y 5.")
            }
        })

        checkBox3.setOnClickListener {
            if (checkBox3.isChecked) {
                checkBox1.isChecked = true
                checkBox4.isChecked = false
                resultText.append("\nOpción 3: Activó la 1 y desactivó la 4.")
            }
        }

        checkBox4.setOnClickListener {
            if (checkBox4.isChecked && !checkBox3.isChecked) {
                checkBox4.isChecked = false
                resultText.append("\nOpción 4: Primero activa la opción 3.")
            }
        }

        checkBox5.setOnClickListener {
            if (checkBox5.isChecked) {
                checkBox1.isChecked = true
                checkBox4.isChecked = false
                resultText.append("\nOpción 5: Activó la 1 y desactivó la 4. Habilito botón")
            }
        }

        checkBox5.setOnCheckedChangeListener(object :
            CompoundButton.OnCheckedChangeListener {
            override fun onCheckedChanged(p0: CompoundButton?, checked: Boolean) {
                if (!checked) {
                    checkButton.isEnabled = false
                } else {
                    checkButton.isEnabled = true
                }
            }

        })
    }

    private fun comprobarCombinacion() {
        if (checkBox1.isChecked &&
            checkBox2.isChecked &&
            checkBox3.isChecked &&
            checkBox4.isChecked &&
            checkBox5.isChecked
        ) {
            resultText.text = "¡Combinación correcta!"
        } else {
            resultText.append("\nCombinación incorrecta. Intenta de nuevo.")
        }
    }
}
```
