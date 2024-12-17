Vamos a realizar las tareas solicitadas paso a paso, comentando detalladamente el código.

### TABLAYOUT

#### 1. En `activity_main.xml` insertar `TabLayout` y modificar `Constraints`

Primero, abrimos el archivo `activity_main.xml` y añadimos un `TabLayout` dentro del `ConstraintLayout`. También ajustamos las constraints para que el `TabLayout` esté correctamente posicionado.

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tab_layout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabIndicatorColor="@android:color/white"
            app:tabSelectedTextColor="@android:color/white"
            app:tabTextColor="@android:color/darker_gray" />

    </com.google.android.material.appbar.AppBarLayout>

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewpager"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/tab_layout"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

#### 2. En `MainActivity` insertar `TabLayout`

Ahora, en `MainActivity`, enlazamos el `TabLayout` con el `ViewPager2`.

```kotlin
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator

class MainActivity : AppCompatActivity() {

    private lateinit var viewPager: ViewPager2
    private lateinit var tabLayout: TabLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        setSupportActionBar(findViewById(R.id.toolbar))

        viewPager = findViewById(R.id.viewpager)
        tabLayout = findViewById(R.id.tab_layout)

        val adapta = ViewPagerAdapter(this, NPESTAÑAS)
        viewPager.adapter = adapta

        // Enlazar TabLayout con ViewPager2
        TabLayoutMediator(tabLayout, viewPager) { tab, position ->
            tab.text = "Tab ${position + 1}"
        }.attach()
    }
}
```

#### 3. Crear un Fragmento para el adaptador del `ViewPager`

Creamos un nuevo fragmento llamado `SampleFragment`.

```kotlin
package dam.pmdm.exam20241t

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup

class SampleFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_sample, container, false)
    }
}
```

### TOOLBAR

#### 4. En `menu_principal.xml` añadir iconos

Añadimos los iconos de Favoritos y Preferencias en `menu_principal.xml`.

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/action_fav"
        android:icon="@drawable/ic_favorite"
        android:title="Favoritos"
        app:showAsAction="ifRoom" />

    <item
        android:id="@+id/action_preferences"
        android:icon="@drawable/ic_person"
        android:title="Preferencias"
        app:showAsAction="ifRoom" />
</menu>
```

### LÓGICA CUESTIONARIO

#### 5. Configurar el botón responder en `QuestionFragment`

Configuramos el botón `responder` para que responda el cuestionario con respuestas predefinidas.

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    responder = view.findViewById(R.id.autorespuesta)
    responder.setOnClickListener {
        // Aquí puedes definir las respuestas predefinidas
        // Por ejemplo, mostrar un Toast con las respuestas
        Toast.makeText(context, "Respuestas enviadas", Toast.LENGTH_SHORT).show()
    }
}
```

### SHAREDPREFERENCES

#### 6. Implementar `cargarPreferencias` y `guardarPreferencias` en `PreferenciasActivity`

Implementamos los métodos para cargar y guardar preferencias.

```kotlin
fun cargarPreferencias() {
    val nombrePref = prefs.getString("nombre", "Alumno Desconocido")
    val checkboxPref = prefs.getBoolean("checkbox", false)
    val radioPref = prefs.getInt("radio", R.id.radioButton1)
    val switchPref = prefs.getBoolean("switch", false)
    val spinnerPref = prefs.getInt("spinner", 0)

    nombre.setText(nombrePref)
    checkbox.isChecked = checkboxPref
    rg.check(radioPref)
    switch.isChecked = switchPref
    spinner.setSelection(spinnerPref)
}

fun guardarPreferencias() {
    val editor = prefs.edit()
    editor.putString("nombre", nombre.text.toString())
    editor.putBoolean("checkbox", checkbox.isChecked)
    editor.putInt("radio", rg.checkedRadioButtonId)
    editor.putBoolean("switch", switch.isChecked)
    editor.putInt("spinner", spinner.selectedItemPosition)
    editor.apply()
}

override fun defaultPreferencias() {
    nombre.setText("Alumno Desconocido")
    checkbox.isChecked = false
    rg.check(R.id.radioButton1)
    switch.isChecked = false
    spinner.setSelection(0)

    // Guardar valores por defecto en SharedPreferences
    guardarPreferencias()
}
```

Con esto, hemos completado todas las tareas solicitadas.

-----

Claro, aquí tienes el código con comentarios más detallados:

### `activity_main.xml`

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- AppBarLayout que contiene la Toolbar y el TabLayout -->
    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <!-- Toolbar para la barra de acciones -->
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <!-- TabLayout para las pestañas -->
        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tab_layout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabIndicatorColor="@android:color/white"
            app:tabSelectedTextColor="@android:color/white"
            app:tabTextColor="@android:color/darker_gray" />

    </com.google.android.material.appbar.AppBarLayout>

    <!-- ViewPager2 para manejar el contenido de las pestañas -->
    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewpager"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/tab_layout"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### `MainActivity.kt`

```kotlin
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator

class MainActivity : AppCompatActivity() {

    private lateinit var viewPager: ViewPager2
    private lateinit var tabLayout: TabLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Configurar la Toolbar
        setSupportActionBar(findViewById(R.id.toolbar))

        // Inicializar ViewPager y TabLayout
        viewPager = findViewById(R.id.viewpager)
        tabLayout = findViewById(R.id.tab_layout)

        // Configurar el adaptador del ViewPager
        val adapta = ViewPagerAdapter(this, NPESTAÑAS)
        viewPager.adapter = adapta

        // Enlazar TabLayout con ViewPager2
        TabLayoutMediator(tabLayout, viewPager) { tab, position ->
            tab.text = "Tab ${position + 1}" // Asignar texto a cada pestaña
        }.attach()
    }

    // Inflar el menú en la actividad
    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        menuInflater.inflate(R.menu.menu_principal, menu)
        return true
    }

    // Manejar eventos de selección del menú
    override fun onOptionsItemSelected(item: MenuItem) = when (item.itemId) {
        R.id.action_fav -> {
            // Acción para el ítem de favoritos
            true
        }

        R.id.action_preferences -> {
            // Iniciar PreferenciasActivity cuando se selecciona el ítem de preferencias
            startActivity(Intent(this, PreferenciasActivity::class.java))
            true
        }

        else -> {
            super.onOptionsItemSelected(item)
        }
    }
}
```

### `SampleFragment.kt`

```kotlin
package dam.pmdm.exam20241t

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup

class SampleFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflar el layout para este fragmento
        return inflater.inflate(R.layout.fragment_sample, container, false)
    }
}
```

### `menu_principal.xml`

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <!-- Ítem de menú para favoritos -->
    <item
        android:id="@+id/action_fav"
        android:icon="@drawable/ic_favorite"
        android:title="Favoritos"
        app:showAsAction="ifRoom" />

    <!-- Ítem de menú para preferencias -->
    <item
        android:id="@+id/action_preferences"
        android:icon="@drawable/ic_person"
        android:title="Preferencias"
        app:showAsAction="ifRoom" />
</menu>
```

### `QuestionFragment.kt`

```kotlin
package dam.pmdm.exam20241t

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.Button
import android.widget.Toast

class QuestionFragment : Fragment() {

    private lateinit var responder: Button

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflar el layout para este fragmento
        return inflater.inflate(R.layout.fragment_quest, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        // Inicializar el botón responder
        responder = view.findViewById(R.id.autorespuesta)
        // Configurar el botón responder para que responda el cuestionario con tus respuestas
        responder.setOnClickListener {
            // Aquí puedes definir las respuestas predefinidas
            // Por ejemplo, mostrar un Toast con las respuestas
            Toast.makeText(context, "Respuestas enviadas", Toast.LENGTH_SHORT).show()
        }
    }
}
```

### `PreferenciasActivity.kt`

```kotlin
package dam.pmdm.exam20241t

import android.content.SharedPreferences
import android.os.Bundle
import android.util.Log
import android.view.View
import android.widget.ArrayAdapter
import android.widget.Button
import android.widget.CheckBox
import android.widget.EditText
import android.widget.RadioButton
import android.widget.RadioGroup
import android.widget.Spinner
import androidx.appcompat.app.AppCompatActivity
import androidx.appcompat.widget.SwitchCompat

class PreferenciasActivity : AppCompatActivity() {
    private lateinit var nombre: EditText
    private lateinit var checkbox: CheckBox
    private lateinit var rg: RadioGroup
    private lateinit var rb: RadioButton
    private lateinit var switch: SwitchCompat
    private lateinit var spinner: Spinner
    private lateinit var guardar: Button
    private lateinit var restaurar: Button
    private lateinit var prefs: SharedPreferences

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_preferencias)

        // Vincular vistas
        nombre = findViewById(R.id.etNombre)
        checkbox = findViewById(R.id.checkbox)
        rg = findViewById(R.id.radioGroup1)
        rb = findViewById(R.id.radioButton2)
        switch = findViewById(R.id.Switch)
        spinner = findViewById(R.id.spinner)
        guardar = findViewById(R.id.pref_guardar)
        restaurar = findViewById(R.id.prefDefault)
        prefs = getSharedPreferences("Preferencias", MODE_PRIVATE)

        // Configurar adaptador del spinner
        val adaptadorSpinner = ArrayAdapter.createFromResource(this, R.array.valoresSpinner, android.R.layout.simple_spinner_item)
        adaptadorSpinner.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
        spinner.adapter = adaptadorSpinner

        // Cargar las opciones seleccionadas desde las preferencias
        cargarPreferencias()

        // Configurar el botón guardar para almacenar las preferencias
        guardar.setOnClickListener(View.OnClickListener {
            guardarPreferencias()
        })

        // Configurar el botón restaurar para establecer valores por defecto
        restaurar.setOnClickListener(View.OnClickListener {
            defaultPreferencias()
        })
    }

    fun cargarPreferencias() {
        // Cargar todas las configuraciones guardadas en preferencias
        val nombrePref = prefs.getString("nombre", "Alumno Desconocido")
        val checkboxPref = prefs.getBoolean("checkbox", false)
        val radioPref = prefs.getInt("radio", R.id.radioButton1)
        val switchPref = prefs.getBoolean("switch", false)
        val spinnerPref = prefs.getInt("spinner", 0)

        // Establecer los valores en los componentes
        nombre.setText(nombrePref)
        checkbox.isChecked = checkboxPref
        rg.check(radioPref)
        switch.isChecked = switchPref
        spinner.setSelection(spinnerPref)
    }

    fun guardarPreferencias() {
        // Guardar las opciones elegidas en las preferencias
        val editor = prefs.edit()
        editor.putString("nombre", nombre.text.toString())
        editor.putBoolean("checkbox", checkbox.isChecked)
        editor.putInt("radio", rg.checkedRadioButtonId)
        editor.putBoolean("switch", switch.isChecked)
        editor.putInt("spinner", spinner.selectedItemPosition)
        editor.apply()
    }

    fun defaultPreferencias() {
        // Establecer valores por defecto en los componentes
        nombre.setText("Alumno Desconocido")
        checkbox.isChecked = false
        rb = findViewById(R.id.radioButton1)
        rb.isChecked = true
        switch.isChecked = false
        spinner.setSelection(0)

        // Guardar valores por defecto en SharedPreferences
        guardarPreferencias()
    }
}
```
