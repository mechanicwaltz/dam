Aquí tienes un desglose del modelo de examen, con cada enunciado acompañado del código correspondiente, todo en formato Markdown:

---

### **1. Crear un `TabLayout` y vincularlo con un `ViewPager2`**
**Enunciado**: Implementa un `TabLayout` en la actividad principal y configúralo para que funcione con un `ViewPager2`.

```kotlin
// MainActivity.kt
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val tabLayout: TabLayout = findViewById(R.id.tabLayout)
    val viewPager: ViewPager2 = findViewById(R.id.viewPager)

    val fragments = listOf(FragmentOne(), FragmentTwo(), FragmentThree())

    val adapter = ViewPagerAdapter(this, fragments)
    viewPager.adapter = adapter

    TabLayoutMediator(tabLayout, viewPager) { tab, position ->
        tab.text = when (position) {
            0 -> "Tareas"
            1 -> "Preferencias"
            2 -> "Colores"
            else -> null
        }
    }.attach()
}

// Adaptador para ViewPager2
class ViewPagerAdapter(
    fragmentActivity: AppCompatActivity,
    private val fragments: List<Fragment>
) : FragmentStateAdapter(fragmentActivity) {
    override fun getItemCount(): Int = fragments.size
    override fun createFragment(position: Int): Fragment = fragments[position]
}
```

---

### **2. Implementar un RecyclerView en un Fragment**
**Enunciado**: En el primer fragmento, añade un `RecyclerView` para mostrar una lista de tareas.

```kotlin
// FragmentOne.kt
class FragmentOne : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = inflater.inflate(R.layout.fragment_one, container, false)

        val recyclerView: RecyclerView = view.findViewById(R.id.recyclerView)
        recyclerView.layoutManager = LinearLayoutManager(context)
        recyclerView.adapter = TaskAdapter(listOf("Tarea 1", "Tarea 2", "Tarea 3"))

        return view
    }
}

class TaskAdapter(private val tasks: List<String>) : RecyclerView.Adapter<TaskAdapter.TaskViewHolder>() {
    class TaskViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val taskText: TextView = view.findViewById(R.id.taskText)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TaskViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_task, parent, false)
        return TaskViewHolder(view)
    }

    override fun onBindViewHolder(holder: TaskViewHolder, position: Int) {
        holder.taskText.text = tasks[position]
    }

    override fun getItemCount(): Int = tasks.size
}
```

---

### **3. Usar `SharedPreferences` para guardar y cargar datos**
**Enunciado**: En el segundo fragmento, guarda un texto en `SharedPreferences` y permite recuperarlo.

```kotlin
// FragmentTwo.kt
class FragmentTwo : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = inflater.inflate(R.layout.fragment_two, container, false)

        val sharedPreferences = requireContext().getSharedPreferences("AppPreferences", Context.MODE_PRIVATE)
        val editor = sharedPreferences.edit()

        val inputField: EditText = view.findViewById(R.id.inputField)
        val saveButton: Button = view.findViewById(R.id.saveButton)
        val loadButton: Button = view.findViewById(R.id.loadButton)

        saveButton.setOnClickListener {
            editor.putString("preferenceKey", inputField.text.toString())
            editor.apply()
        }

        loadButton.setOnClickListener {
            val loadedValue = sharedPreferences.getString("preferenceKey", "Valor por defecto")
            inputField.setText(loadedValue)
        }

        return view
    }
}
```

---

### **4. Cambiar el color de fondo dinámicamente**
**Enunciado**: En el tercer fragmento, añade un botón que cambie el color de fondo de forma aleatoria.

```kotlin
// FragmentThree.kt
class FragmentThree : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = inflater.inflate(R.layout.fragment_three, container, false)

        val changeColorButton: Button = view.findViewById(R.id.changeColorButton)
        changeColorButton.setOnClickListener {
            val randomColor = Random.nextInt(0xFFFFFF) or (0xFF shl 24)
            view.setBackgroundColor(randomColor)
        }

        return view
    }
}
```

---

### **5. Diseñar los layouts correspondientes**
**Enunciado**: Crea los layouts XML para cada fragmento y la actividad principal.

#### **`activity_main.xml`**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

#### **`fragment_one.xml`**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

#### **`fragment_two.xml`**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/inputField"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Escribe algo" />

    <Button
        android:id="@+id/saveButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Guardar" />

    <Button
        android:id="@+id/loadButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cargar" />
</LinearLayout>
```

#### **`fragment_three.xml`**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <Button
        android:id="@+id/changeColorButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cambiar Color" />
</LinearLayout>
```

#### **`item_task.xml`**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="8dp">

    <TextView
        android:id="@+id/taskText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp" />
</LinearLayout>
```

---

// MainActivity.kt

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.viewpager2.adapter.FragmentStateAdapter
import androidx.viewpager2.widget.ViewPager2
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator

// Actividad principal
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Referencia al TabLayout
        val tabLayout: TabLayout = findViewById(R.id.tabLayout)
        // Referencia al ViewPager2
        val viewPager: ViewPager2 = findViewById(R.id.viewPager)

        // Crear la lista de fragmentos que queremos mostrar
        val fragments = listOf(
            FragmentOne(), // Primer fragmento
            FragmentTwo(), // Segundo fragmento
            FragmentThree() // Tercer fragmento
        )

        // Configurar el adaptador para el ViewPager2
        val adapter = ViewPagerAdapter(this, fragments)
        viewPager.adapter = adapter

        // Vincular el TabLayout con el ViewPager2 usando TabLayoutMediator
        TabLayoutMediator(tabLayout, viewPager) { tab, position ->
            // Asignar un texto a cada pestaña según su posición
            tab.text = when (position) {
                0 -> "Tareas" // Nombre de la primera pestaña
                1 -> "Preferencias" // Nombre de la segunda pestaña
                2 -> "Colores" // Nombre de la tercera pestaña
                else -> null
            }
        }.attach() // Importante: .attach() conecta el TabLayout y el ViewPager2
    }
}

// Adaptador para el ViewPager2
class ViewPagerAdapter(
    fragmentActivity: AppCompatActivity, // Contexto de la actividad principal
    private val fragments: List<Fragment> // Lista de fragmentos
) : FragmentStateAdapter(fragmentActivity) {

    // Devuelve la cantidad de fragmentos
    override fun getItemCount(): Int = fragments.size

    // Devuelve el fragmento correspondiente a la posición
    override fun createFragment(position: Int): Fragment = fragments[position]
}

// FragmentOne.kt
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

// Primer fragmento
class FragmentOne : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflar el diseño para este fragmento
        val view = inflater.inflate(R.layout.fragment_one, container, false)

        val recyclerView: RecyclerView = view.findViewById(R.id.recyclerView)
        recyclerView.layoutManager = LinearLayoutManager(context)
        recyclerView.adapter = TaskAdapter(listOf("Tarea 1", "Tarea 2", "Tarea 3"))

        return view
    }
}

class TaskAdapter(private val tasks: List<String>) : RecyclerView.Adapter<TaskAdapter.TaskViewHolder>() {
    class TaskViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val taskText: android.widget.TextView = view.findViewById(R.id.taskText)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TaskViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_task, parent, false)
        return TaskViewHolder(view)
    }

    override fun onBindViewHolder(holder: TaskViewHolder, position: Int) {
        holder.taskText.text = tasks[position]
    }

    override fun getItemCount(): Int = tasks.size
}

// FragmentTwo.kt
import android.content.Context
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.Button
import android.widget.EditText
import androidx.fragment.app.Fragment

// Segundo fragmento
class FragmentTwo : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = inflater.inflate(R.layout.fragment_two, container, false)

        val sharedPreferences = requireContext().getSharedPreferences("AppPreferences", Context.MODE_PRIVATE)
        val editor = sharedPreferences.edit()

        val inputField: EditText = view.findViewById(R.id.inputField)
        val saveButton: Button = view.findViewById(R.id.saveButton)
        val loadButton: Button = view.findViewById(R.id.loadButton)

        saveButton.setOnClickListener {
            editor.putString("preferenceKey", inputField.text.toString())
            editor.apply()
        }

        loadButton.setOnClickListener {
            val loadedValue = sharedPreferences.getString("preferenceKey", "Valor por defecto")
            inputField.setText(loadedValue)
        }

        return view
    }
}

// FragmentThree.kt
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import kotlin.random.Random

// Tercer fragmento
class FragmentThree : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = inflater.inflate(R.layout.fragment_three, container, false)

        val changeColorButton: android.widget.Button = view.findViewById(R.id.changeColorButton)
        changeColorButton.setOnClickListener {
            val randomColor = Random.nextInt(0xFFFFFF) or (0xFF shl 24)
            view.setBackgroundColor(randomColor)
        }

        return view
    }
}

// Layout: activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!-- TabLayout para mostrar las pestañas -->
    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabIndicatorColor="@color/purple_500"
        app:tabSelectedTextColor="@color/purple_500"
        app:tabTextColor="@color/black" />

    <!-- ViewPager2 para el contenido deslizable -->
    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>

// Layout: fragment_one.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>

// Layout: fragment_two.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/inputField"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Escribe algo" />

    <Button
        android:id="@+id/saveButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Guardar" />

    <Button
        android:id="@+id/loadButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cargar" />
</LinearLayout>

// Layout: fragment_three.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/changeColorButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cambiar Color" />
</LinearLayout>

// Layout: item_task.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="8dp">

    <TextView
        android:id="@+id/taskText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp" />
</LinearLayout>
