# SharedPreferences:
Este código define una actividad principal (`MainActivity`) en una aplicación Android que utiliza `SharedPreferences` para guardar y cargar datos de preferencias del usuario.

### Detalles del código:
1. **Declaración de variables**:
   - Se declaran variables para los elementos de la interfaz de usuario: `Button`, `EditText`, `MaterialSwitch` y `CheckBox`.

2. **Método `onCreate`**:
   - **Configuración de la vista**: Se establece el contenido de la vista con `setContentView(R.layout.activity_main)`.
   - **Vinculación de vistas**: Se vinculan las vistas de la interfaz de usuario con sus respectivos IDs.
   - **Configuración del botón `guardar`**:
     - Se establece un `OnClickListener` para el botón `guardar`.
     - Al hacer clic, se obtienen las `SharedPreferences` y se crea un editor.
     - Se guardan los estados del `Switch`, `CheckBox` y el texto del `EditText` en las preferencias.
     - Se aplica el editor para guardar los cambios.
   - **Carga de datos por defecto**:
     - Se obtienen las `SharedPreferences` y se cargan los valores guardados previamente.
     - Se establecen los valores cargados en los elementos de la interfaz de usuario.

### Funcionamiento de `SharedPreferences`:
- **Obtener `SharedPreferences`**: Se utiliza `getSharedPreferences("preferencias", MODE_PRIVATE)` para obtener un objeto `SharedPreferences` con el nombre "preferencias".
- **Guardar datos**:
  - Se crea un editor con `prefs.edit()`.
  - Se utilizan métodos como `putBoolean` y `putString` para agregar datos al editor.
  - Se aplica el editor con `editor.apply()` para guardar los cambios de manera asíncrona (o `editor.commit()` para guardar de manera síncrona).
- **Cargar datos**:
  - Se utilizan métodos como `getBoolean` y `getString` para obtener los valores guardados en las preferencias.

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var guardar: Button
    private lateinit var editText: EditText
    private lateinit var sw: MaterialSwitch
    private lateinit var checkBox: CheckBox

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // bind
        guardar = findViewById(R.id.guardar)
        editText = findViewById(R.id.editText)
        sw = findViewById(R.id.switchh)
        checkBox = findViewById(R.id.checkbox)

        guardar.setOnClickListener {
            val prefs: SharedPreferences = getSharedPreferences("preferencias", MODE_PRIVATE)
            val editor = prefs.edit()
            editor.putBoolean("switch", sw.isChecked)
            editor.putBoolean("checkbox", checkBox.isChecked)
            editor.putString("texto", editText.text.toString())
            editor.apply()
        }

        // Carga de datos por defecto
        val defaultPrefs = getSharedPreferences("preferencias", MODE_PRIVATE)
        editText.setText(defaultPrefs.getString("texto", "No has guardado nada"))
        checkBox.isChecked = defaultPrefs.getBoolean("checkbox", true)
        sw.isChecked = defaultPrefs.getBoolean("switch", true)
    }
}
```

Las `SharedPreferences` se almacenan en un archivo XML en el almacenamiento interno de la aplicación. No puedes ver directamente este archivo desde el dispositivo sin acceso root. Sin embargo, puedes verificar que los datos se han guardado correctamente cargándolos de nuevo en la aplicación, como se hace en el método `onCreate` de tu `MainActivity`.

Para depurar y verificar los valores guardados, puedes agregar logs en tu código:

```kotlin
import android.util.Log

class MainActivity : AppCompatActivity() {
    // ... (rest of your code)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // bind
        guardar = findViewById(R.id.guardar)
        editText = findViewById(R.id.editText)
        sw = findViewById(R.id.switchh)
        checkBox = findViewById(R.id.checkbox)

        guardar.setOnClickListener {
            val prefs: SharedPreferences = getSharedPreferences("preferencias", MODE_PRIVATE)
            val editor = prefs.edit()
            editor.putBoolean("switch", sw.isChecked)
            editor.putBoolean("checkbox", checkBox.isChecked)
            editor.putString("texto", editText.text.toString())
            editor.apply()

            // Log the saved values
            Log.d("MainActivity", "Saved switch: ${sw.isChecked}, checkbox: ${checkBox.isChecked}, texto: ${editText.text}")
        }

        // Carga de datos por defecto
        val defaultPrefs = getSharedPreferences("preferencias", MODE_PRIVATE)
        val savedText = defaultPrefs.getString("texto", "No has guardado nada")
        val savedCheckbox = defaultPrefs.getBoolean("checkbox", true)
        val savedSwitch = defaultPrefs.getBoolean("switch", true)

        editText.setText(savedText)
        checkBox.isChecked = savedCheckbox
        sw.isChecked = savedSwitch

        // Log the loaded values
        Log.d("MainActivity", "Loaded switch: $savedSwitch, checkbox: $savedCheckbox, texto: $savedText")
    }
}
```

Puedes ver estos logs en Logcat en Android Studio para verificar los valores guardados y cargados.
