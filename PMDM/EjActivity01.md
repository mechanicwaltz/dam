Claro, aquí tienes una explicación detallada de cada clase en tu aplicación, junto con comentarios en el código.

### `MainActivity`

La clase `MainActivity` gestiona la pantalla principal de la aplicación. Permite al usuario ingresar y guardar datos de personas, y muestra una lista de personas en un `RecyclerView`.

```kotlin
package dam.pmdm.ejactivity01

import android.app.Activity
import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import androidx.activity.result.ActivityResultLauncher
import androidx.activity.result.contract.ActivityResultContracts
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : AppCompatActivity() {
    private lateinit var nameEditText: EditText
    private lateinit var ageEditText: EditText
    private lateinit var guardarButton: Button
    private lateinit var recyclerView: RecyclerView
    private lateinit var startForResult: ActivityResultLauncher<Intent>
    private lateinit var personaAdapter: PersonaAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Vincular componentes de la interfaz de usuario
        nameEditText = findViewById(R.id.nameEditText)
        ageEditText = findViewById(R.id.ageEditText)
        guardarButton = findViewById(R.id.guardarButton)
        recyclerView = findViewById(R.id.recyclerView)

        // Inicializar adaptador y asignarlo al RecyclerView
        personaAdapter = PersonaAdapter()
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = personaAdapter

        // Registrar ActivityResultLauncher para manejar resultados de EditActivity
        startForResult = registerForActivityResult(
            ActivityResultContracts.StartActivityForResult()
        ) { result ->
            if (result.resultCode == Activity.RESULT_OK) {
                val data = result.data
                if (data != null) {
                    val position = data.getIntExtra("position", -1)
                    val updatedName = data.getStringExtra("updatedName")
                    val updatedAge = data.getIntExtra("updatedAge", -1)

                    if (position != -1 && updatedName != null && updatedAge != -1) {
                        val p = Persona(updatedName, updatedAge)
                        personaAdapter.changeItem(position, p)
                    }
                }
            }
        }

        // Establecer listener para el botón de guardar
        guardarButton.setOnClickListener {
            val name = nameEditText.text.toString()
            val age = ageEditText.text.toString().toIntOrNull() ?: 0
            personaAdapter.addpersona(Persona(name, age))

            nameEditText.text.clear()
            ageEditText.text.clear()
        }

        // Implementar callback para clics en elementos del adaptador
        personaAdapter.setAdaptadorCallback(object : PersonaAdapter.PersonaAdapterCallback {
            override fun onClickPersona(p: Persona, position: Int) {
                val intent = Intent(this@MainActivity, EditActivity::class.java).apply {
                    putExtra("position", position)
                    putExtra("name", p.name)
                    putExtra("age", p.age)
                }
                startForResult.launch(intent)
            }
        })
    }
}
```

### `EditActivity`

La clase `EditActivity` permite al usuario editar los datos de una persona seleccionada y devolver los datos actualizados a `MainActivity`.

```kotlin
package dam.pmdm.ejactivity01

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import androidx.appcompat.app.AppCompatActivity

class EditActivity : AppCompatActivity() {
    private lateinit var nameEditText: EditText
    private lateinit var ageEditText: EditText
    private lateinit var saveButton: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_edit)

        // Vincular componentes de la interfaz de usuario
        nameEditText = findViewById(R.id.editNameEditText)
        ageEditText = findViewById(R.id.editAgeEditText)
        saveButton = findViewById(R.id.saveButton)

        // Recuperar datos del Intent
        val pos = intent.getIntExtra("position", 0)
        val name = intent.getStringExtra("name")
        val age = intent.getIntExtra("age", -1)

        // Mostrar datos en los campos de texto
        nameEditText.setText(name)
        ageEditText.setText(age.toString())

        // Listener para guardar cambios
        saveButton.setOnClickListener {
            val updatedName = nameEditText.text.toString()
            val updatedAge = ageEditText.text.toString().toIntOrNull() ?: 0

            // Crear Intent para devolver los datos actualizados
            val resultIntent = Intent().apply {
                putExtra("position", pos)
                putExtra("updatedName", updatedName)
                putExtra("updatedAge", updatedAge)
            }

            setResult(RESULT_OK, resultIntent)
            finish() // Cerrar la actividad
        }
    }
}
```

### `Persona`

La clase `Persona` es un modelo de datos que representa a una persona con un nombre y una edad.

```kotlin
package dam.pmdm.ejactivity01

data class Persona(
    var name: String,
    var age: Int
)
```

### `PersonaAdapter`

La clase `PersonaAdapter` es un adaptador para el `RecyclerView` que gestiona la lista de objetos `Persona`.

```kotlin
package dam.pmdm.ejactivity01

import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView

class PersonaAdapter() : RecyclerView.Adapter<PersonaViewHolder>() {
    private val personList: MutableList<Persona> = mutableListOf()
    private lateinit var listener: PersonaAdapterCallback

    interface PersonaAdapterCallback {
        fun onClickPersona(p: Persona, position: Int)
    }

    fun setAdaptadorCallback(listener: PersonaAdapterCallback) {
        this.listener = listener
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PersonaViewHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.fila_persona, parent, false)
        return PersonaViewHolder(view)
    }

    override fun getItemCount() = personList.size

    override fun onBindViewHolder(holder: PersonaViewHolder, position: Int) {
        val person = personList[position]
        holder.nameTextView.text = person.name
        holder.ageTextView.text = person.age.toString()

        // Listener para el clic
        holder.itemView.setOnClickListener {
            listener.onClickPersona(person, position)
        }
    }

    fun changeItem(position: Int, p: Persona) {
        personList[position] = p
        notifyItemChanged(position)
    }

    fun addpersona(persona: Persona) {
        personList.add(persona)
        notifyItemInserted(personList.size - 1)
    }
}
```

### `PersonaViewHolder`

La clase `PersonaViewHolder` es un `ViewHolder` para el `RecyclerView` que contiene las vistas para mostrar los datos de una persona.

```kotlin
package dam.pmdm.ejactivity01

import android.view.View
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView

class PersonaViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    var nameTextView: TextView = itemView.findViewById(R.id.nameTextView)
    var ageTextView: TextView = itemView.findViewById(R.id.ageTextView)
}
```


```
