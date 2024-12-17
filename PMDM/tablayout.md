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
                0 -> "Tab 1" // Nombre de la primera pestaña
                1 -> "Tab 2" // Nombre de la segunda pestaña
                2 -> "Tab 3" // Nombre de la tercera pestaña
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

// Primer fragmento
class FragmentOne : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflar el diseño para este fragmento
        return inflater.inflate(R.layout.fragment_one, container, false)
    }
}

// FragmentTwo.kt
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

// Segundo fragmento
class FragmentTwo : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflar el diseño para este fragmento
        return inflater.inflate(R.layout.fragment_two, container, false)
    }
}

// FragmentThree.kt
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

// Tercer fragmento
class FragmentThree : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflar el diseño para este fragmento
        return inflater.inflate(R.layout.fragment_three, container, false)
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
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Fragment One"
        android:textSize="20sp" />

</FrameLayout>

// Layout: fragment_two.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Fragment Two"
        android:textSize="20sp" />

</FrameLayout>

// Layout: fragment_three.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Fragment Three"
        android:textSize="20sp" />

</FrameLayout>




¡Claro! En Android Studio, puedes usar **TabLayout** junto con **ViewPager2** para crear interfaces con pestañas (tabs) deslizables. Aquí te explico cómo funciona y cómo implementarlo usando **Kotlin**:

---

## 1. **Qué es TabLayout y ViewPager2**
- **TabLayout**: Un componente que permite mostrar pestañas que los usuarios pueden seleccionar.
- **ViewPager2**: Un widget que permite deslizar contenido entre diferentes fragmentos o vistas. Es una versión mejorada de ViewPager con más funcionalidades y mejor rendimiento.

Al combinar **TabLayout** y **ViewPager2**, puedes sincronizar las pestañas con el contenido deslizable.

---

## 2. **Pasos para implementarlo**

### 1. **Agrega las dependencias necesarias**
En tu archivo `build.gradle` del módulo de tu proyecto, asegúrate de incluir las siguientes dependencias:

```gradle
implementation 'com.google.android.material:material:1.9.0' // Para TabLayout
implementation 'androidx.viewpager2:viewpager2:1.0.0'       // Para ViewPager2
```

### 2. **Diseña el layout principal**
Define un archivo de diseño XML con el `TabLayout` y el `ViewPager2`.

```xml
<!-- res/layout/activity_main.xml -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!-- TabLayout para las pestañas -->
    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabIndicatorColor="@color/purple_500"
        app:tabSelectedTextColor="@color/purple_500"
        app:tabTextColor="@color/black" />

    <!-- ViewPager2 para el contenido -->
    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

### 3. **Crea los fragmentos**
Crea los fragmentos que se mostrarán en cada pestaña.

```kotlin
// Fragmento 1
class FragmentOne : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        return inflater.inflate(R.layout.fragment_one, container, false)
    }
}

// Fragmento 2
class FragmentTwo : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        return inflater.inflate(R.layout.fragment_two, container, false)
    }
}

// Fragmento 3
class FragmentThree : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        return inflater.inflate(R.layout.fragment_three, container, false)
    }
}
```

### 4. **Configura el adaptador del ViewPager2**
Crea un adaptador para manejar los fragmentos.

```kotlin
class ViewPagerAdapter(
    fragmentActivity: FragmentActivity,
    private val fragments: List<Fragment>
) : FragmentStateAdapter(fragmentActivity) {

    override fun getItemCount(): Int = fragments.size

    override fun createFragment(position: Int): Fragment = fragments[position]
}
```

### 5. **Conecta el TabLayout con el ViewPager2**
Configura el `TabLayout` y el `ViewPager2` en tu `MainActivity`.

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val tabLayout: TabLayout = findViewById(R.id.tabLayout)
        val viewPager: ViewPager2 = findViewById(R.id.viewPager)

        // Lista de fragmentos
        val fragments = listOf(
            FragmentOne(),
            FragmentTwo(),
            FragmentThree()
        )

        // Configurar adaptador
        val adapter = ViewPagerAdapter(this, fragments)
        viewPager.adapter = adapter

        // Conectar TabLayout con ViewPager2
        TabLayoutMediator(tabLayout, viewPager) { tab, position ->
            tab.text = when (position) {
                0 -> "Tab 1"
                1 -> "Tab 2"
                2 -> "Tab 3"
                else -> null
            }
        }.attach()
    }
}
```

---

## 3. **Cómo funciona**
1. **TabLayout** actúa como el selector de pestañas visible para el usuario.
2. **ViewPager2** maneja el contenido deslizable y se sincroniza con las pestañas.
3. **TabLayoutMediator** conecta el `TabLayout` y el `ViewPager2`, permitiendo que los cambios en uno se reflejen en el otro.

---

¡Y listo! Ahora tendrás un diseño con pestañas deslizables totalmente funcional. Si tienes más preguntas o necesitas personalización, ¡avísame! 😊
