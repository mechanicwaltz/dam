Claro, aquí tienes los pasos para implementar un `Toolbar` en una aplicación Android:

1. **Agregar el Toolbar al layout XML**:
   Abre el archivo de layout XML donde deseas agregar el `Toolbar` (por ejemplo, `activity_main.xml`) y añade el siguiente código dentro del `CoordinatorLayout` o `AppBarLayout`:

   ```xml
   <androidx.appcompat.widget.Toolbar
       android:id="@+id/toolbar"
       android:layout_width="match_parent"
       android:layout_height="?attr/actionBarSize"
       android:background="?attr/colorPrimary"
       android:elevation="4dp"
       android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
       app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
   ```

2. **Configurar el Toolbar en la Activity**:
   En tu `Activity` (por ejemplo, `MainActivity`), configura el `Toolbar` en el método `onCreate`:

   ```kotlin
   import android.os.Bundle
   import androidx.appcompat.app.AppCompatActivity
   import androidx.appcompat.widget.Toolbar

   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)

           // Configurar el Toolbar
           val toolbar: Toolbar = findViewById(R.id.toolbar)
           setSupportActionBar(toolbar)
       }
   }
   ```

3. **Agregar un menú al Toolbar**:
   Crea un archivo de menú XML en el directorio `res/menu` (por ejemplo, `menu_main.xml`) y define los elementos del menú:

   ```xml
   <menu xmlns:android="http://schemas.android.com/apk/res/android">
       <item
           android:id="@+id/action_settings"
           android:title="Settings"
           android:orderInCategory="100"
           android:showAsAction="never" />
   </menu>
   ```

4. **Inflar el menú en la Activity**:
   En tu `Activity`, sobrescribe el método `onCreateOptionsMenu` para inflar el menú:

   ```kotlin
   override fun onCreateOptionsMenu(menu: Menu?): Boolean {
       menuInflater.inflate(R.menu.menu_main, menu)
       return true
   }
   ```

5. **Manejar eventos del menú**:
   Sobrescribe el método `onOptionsItemSelected` para manejar los eventos de los elementos del menú:

   ```kotlin
   override fun onOptionsItemSelected(item: MenuItem): Boolean {
       return when (item.itemId) {
           R.id.action_settings -> {
               // Manejar la acción de configuración
               true
           }
           else -> super.onOptionsItemSelected(item)
       }
   }
   ```

Con estos pasos, habrás implementado un `Toolbar` en tu aplicación.

Para añadir botones a tu `Toolbar`, sigue estos pasos:

1. **Agregar los botones al menú XML**:
   Abre el archivo de menú XML (por ejemplo, `menu_main.xml`) y define los elementos del menú que representarán los botones:

   ```xml
   <menu xmlns:android="http://schemas.android.com/apk/res/android">
       <item
           android:id="@+id/action_add"
           android:icon="@drawable/ic_add"
           android:title="Add"
           android:orderInCategory="100"
           android:showAsAction="always" />
       <item
           android:id="@+id/action_settings"
           android:title="Settings"
           android:orderInCategory="200"
           android:showAsAction="never" />
   </menu>
   ```

2. **Inflar el menú en la Activity**:
   En tu `Activity`, sobrescribe el método `onCreateOptionsMenu` para inflar el menú:

   ```kotlin
   override fun onCreateOptionsMenu(menu: Menu?): Boolean {
       menuInflater.inflate(R.menu.menu_main, menu)
       return true
   }
   ```

3. **Manejar eventos de los botones**:
   Sobrescribe el método `onOptionsItemSelected` para manejar los eventos de los elementos del menú:

   ```kotlin
   override fun onOptionsItemSelected(item: MenuItem): Boolean {
       return when (item.itemId) {
           R.id.action_add -> {
               // Manejar la acción de agregar
               true
           }
           R.id.action_settings -> {
               // Manejar la acción de configuración
               true
           }
           else -> super.onOptionsItemSelected(item)
       }
   }
   ```

Con estos pasos, habrás añadido botones a tu `Toolbar` y manejado sus eventos.

Claro, aquí tienes la respuesta en español:

Para hacer que los botones en el `Toolbar` controlen `Fragments`, sigue estos pasos:

1. **Crear los Fragments**:
   Define las clases `Fragment` que deseas controlar. Por ejemplo, `FragmentA` y `FragmentB`.

   ```kotlin
   // FragmentA.kt
   package dam.pmdm.ejactivity01

   import android.os.Bundle
   import android.view.LayoutInflater
   import android.view.View
   import android.view.ViewGroup
   import androidx.fragment.app.Fragment

   class FragmentA : Fragment() {
       override fun onCreateView(
           inflater: LayoutInflater, container: ViewGroup?,
           savedInstanceState: Bundle?
       ): View? {
           return inflater.inflate(R.layout.fragment_a, container, false)
       }
   }
   ```

   ```kotlin
   // FragmentB.kt
   package dam.pmdm.ejactivity01

   import android.os.Bundle
   import android.view.LayoutInflater
   import android.view.View
   import android.view.ViewGroup
   import androidx.fragment.app.Fragment

   class FragmentB : Fragment() {
       override fun onCreateView(
           inflater: LayoutInflater, container: ViewGroup?,
           savedInstanceState: Bundle?
       ): View? {
           return inflater.inflate(R.layout.fragment_b, container, false)
       }
   }
   ```

2. **Agregar los Fragments al layout**:
   Define un `FrameLayout` en tu `activity_main.xml` para alojar los `Fragments`.

   ```xml
   <!-- activity_main.xml -->
   <FrameLayout
       android:id="@+id/fragment_container"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:layout_below="@id/toolbar" />
   ```

3. **Manejar transacciones de Fragment en la Activity**:
   En tu `MainActivity`, maneja las transacciones de `Fragment` cuando se hagan clics en los botones.

   ```kotlin
   import android.os.Bundle
   import android.view.Menu
   import android.view.MenuItem
   import androidx.appcompat.app.AppCompatActivity
   import androidx.appcompat.widget.Toolbar
   import androidx.fragment.app.Fragment

   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)

           // Configurar el Toolbar
           val toolbar: Toolbar = findViewById(R.id.toolbar)
           setSupportActionBar(toolbar)
       }

       override fun onCreateOptionsMenu(menu: Menu?): Boolean {
           menuInflater.inflate(R.menu.menu_main, menu)
           return true
       }

       override fun onOptionsItemSelected(item: MenuItem): Boolean {
           return when (item.itemId) {
               R.id.action_add -> {
                   replaceFragment(FragmentA())
                   true
               }
               R.id.action_settings -> {
                   replaceFragment(FragmentB())
                   true
               }
               else -> super.onOptionsItemSelected(item)
           }
       }

       private fun replaceFragment(fragment: Fragment) {
           supportFragmentManager.beginTransaction()
               .replace(R.id.fragment_container, fragment)
               .addToBackStack(null)
               .commit()
       }
   }
   ```

Con estos pasos, los botones en tu `Toolbar` controlarán la visualización de `Fragments` en tu `Activity`.


Para pasar datos entre fragmentos, puedes usar un `ViewModel` compartido entre los fragmentos o usar argumentos de fragmento. Aquí tienes un ejemplo usando argumentos de fragmento:

1. **Crear el primer fragmento y establecer argumentos**:

```kotlin
package dam.pmdm.ejactivity01

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import kotlinx.android.synthetic.main.fragment_a.*

class FragmentA : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_a, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        button_send.setOnClickListener {
            val bundle = Bundle().apply {
                putString("key", editText_data.text.toString())
            }
            findNavController().navigate(R.id.action_fragmentA_to_fragmentB, bundle)
        }
    }
}
```

2. **Recuperar los argumentos en el segundo fragmento**:

```kotlin
package dam.pmdm.ejactivity01

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import kotlinx.android.synthetic.main.fragment_b.*

class FragmentB : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_b, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        val data = arguments?.getString("key")
        textView_data.text = data
    }
}
```

3. **Definir la acción de navegación en `nav_graph.xml`**:

```xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@id/fragmentA">

    <fragment
        android:id="@+id/fragmentA"
        android:name="dam.pmdm.ejactivity01.FragmentA"
        tools:layout="@layout/fragment_a">
        <action
            android:id="@+id/action_fragmentA_to_fragmentB"
            app:destination="@id/fragmentB" />
    </fragment>

    <fragment
        android:id="@+id/fragmentB"
        android:name="dam.pmdm.ejactivity01.FragmentB"
        tools:layout="@layout/fragment_b" />
</navigation>
```

Con estos pasos, puedes pasar datos entre fragmentos usando argumentos de fragmento.
