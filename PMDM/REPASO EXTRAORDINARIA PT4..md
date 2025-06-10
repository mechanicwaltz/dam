
# Fragmento Configurador

En esta lección configuraremos el fragmento **Configurador** para:

- Cargar valores de `SharedPreferences` al iniciar.
- Modificar el número de alumno para que acepte solo números.
- Al pulsar el botón, guardar los valores en `SharedPreferences`.
- Guardar el ID de la provincia como `String` y el número de alumno.

---

## Modificar el layout

![image](https://github.com/user-attachments/assets/8eb74730-4e0e-4a74-a35d-c224d30908cc)


El layout `fragment_configurador.xml` debe tener:

- Un `TextView` para el título **CONFIGURACIÓN** (`tvConfiguracion`).
- Un `TextView` para el título del número de alumno (`tvNumeroAlu`).
- Un `EditText` para ingresar el número de alumno (`etNumeroAlu`), con entrada numérica.
- Un `Spinner` para la lista desplegable de provincias (`spinner`).
- Un `Button` para guardar en `SharedPreferences` (`SPGuardar`), con ícono de guardar.

Utilizaremos `ConstraintLayout` para organizar los elementos.

---

## Código XML del layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@android:id/widget_frame"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F8F4E3"
    android:orientation="vertical">

    <TextView
        android:id="@+id/tvConfiguracion"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:background="#706C61"
        android:text="@string/configurador"
        android:textColor="@color/white"
        android:textSize="30sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/tvNumeroAlu"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginTop="20dp"
        android:text="@string/nombre"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tvConfiguracion" />

    <EditText
        android:id="@+id/etNumeroAlu"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:inputType="number"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tvNumeroAlu" />

    <Spinner
        android:id="@+id/spinner"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginTop="24dp"
        android:layout_marginEnd="20dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/etNumeroAlu" />

    <Button
        android:id="@+id/SPGuardar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="112dp"
        android:background="#706C61"
        android:drawableEnd="@drawable/save"
        android:text="@string/guardar"
        android:textColor="@color/white"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@id/spinner" />

</androidx.constraintlayout.widget.ConstraintLayout>
````
![image](https://github.com/user-attachments/assets/fca01c7e-dd94-43ba-b2f7-4a63cb5d9861)

---

## Notas

* En el `EditText` se ha añadido `android:inputType="number"` para aceptar solo números.
* El botón incluye un drawable (icono) de guardar, que debes colocar en `res/drawable/save.xml` o importarlo como recurso vectorial.
* Los textos `@string/configurador`, `@string/nombre` y `@string/guardar` deben estar definidos en `strings.xml`.
* El fondo del layout es color `#F8F4E3`.
* El botón tiene fondo color `#706C61` con texto blanco.

---

## Ajustar la altura del ViewPager

Para que el fragmento ocupe el espacio disponible correctamente, modifica el `ViewPager2` en el `activity_main.xml` y cambia la altura a `0dp` para que use constraints y se adapte al espacio libre.

```xml
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="0dp"
    app:layout_constraintTop_toBottomOf="@id/tabLayout"
    app:layout_constraintBottom_toBottomOf="parent" />
```

Así, el fragmento Configurador se ajustará a la pantalla sin depender solo de su contenido.
![image](https://github.com/user-attachments/assets/fffd677f-c45f-40a5-a403-a49294442fa1)

---

# Enlazar widgets en el fragmento ConfiguradorFragment

Para referenciar los widgets del layout (`EditText`, `Spinner` y `Button`) en el fragmento, declaramos variables `lateinit` para cada uno:

```kotlin
private lateinit var nAlu: EditText
private lateinit var spinner: Spinner
private lateinit var guardar: Button
````

---

## Ciclo de vida del fragmento

* El método `onCreateView` se usa para inflar la vista del fragmento.
* Para inicializar los widgets referenciando las vistas infladas, debemos usar `onViewCreated`, que se llama justo después de que la vista ha sido creada.

---

## Código Kotlin en ConfiguradorFragment

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // Enlazamos las variables con los widgets del layout
    nAlu = view.findViewById(R.id.etNumeroAlu)
    spinner = view.findViewById(R.id.spinner)
    guardar = view.findViewById(R.id.SPGuardar)
}
```

---

Así, podrás manejar eventos o modificar estos widgets desde tu fragmento.


---
# Configurar el Spinner con una lista de provincias

En nuestro fragmento `ConfiguradorFragment`, vamos a mostrar una lista de provincias en el spinner.

---

## Clase Provincia

Primero, definimos la clase `Provincia` que contiene el `id` y el `nombre`. Sobrescribimos `toString()` para mostrar solo el nombre en el spinner.

```kotlin
data class Provincia(val id: String, val nombre: String) {
    override fun toString(): String {
        return nombre
    }
}
````

---

## Método para generar la lista de provincias

Creamos un método que devuelve un array con todas las provincias:

```kotlin
private fun generadorProvincias(): Array<Provincia> {
    return arrayOf(
        Provincia("15", "A Coruña"),
        Provincia("03", "Alacant/Alicante"),
        Provincia("02", "Albacete"),
        Provincia("04", "Almería"),
        Provincia("01", "Araba/Álava"),
        Provincia("33", "Asturias"),
        Provincia("05", "Ávila"),
        Provincia("06", "Badajoz"),
        Provincia("08", "Barcelona"),
        Provincia("48", "Bizkaia"),
        Provincia("09", "Burgos"),
        Provincia("10", "Cáceres"),
        Provincia("11", "Cádiz"),
        Provincia("39", "Cantabria"),
        Provincia("12", "Castelló/Castellón"),
        Provincia("51", "Ceuta"),
        Provincia("13", "Ciudad Real"),
        Provincia("14", "Córdoba"),
        Provincia("16", "Cuenca"),
        Provincia("20", "Gipuzkoa"),
        Provincia("17", "Girona"),
        Provincia("18", "Granada"),
        Provincia("19", "Guadalajara"),
        Provincia("21", "Huelva"),
        Provincia("22", "Huesca"),
        Provincia("07", "Illes Balears"),
        Provincia("23", "Jaén"),
        Provincia("26", "La Rioja"),
        Provincia("35", "Las Palmas"),
        Provincia("24", "León"),
        Provincia("25", "Lleida"),
        Provincia("27", "Lugo"),
        Provincia("28", "Madrid"),
        Provincia("29", "Málaga"),
        Provincia("52", "Melilla"),
        Provincia("30", "Murcia"),
        Provincia("31", "Navarra"),
        Provincia("32", "Ourense"),
        Provincia("34", "Palencia"),
        Provincia("36", "Pontevedra"),
        Provincia("37", "Salamanca"),
        Provincia("38", "Santa Cruz de Tenerife"),
        Provincia("40", "Segovia"),
        Provincia("41", "Sevilla"),
        Provincia("42", "Soria"),
        Provincia("43", "Tarragona"),
        Provincia("44", "Teruel"),
        Provincia("45", "Toledo"),
        Provincia("46", "València/Valencia"),
        Provincia("47", "Valladolid"),
        Provincia("49", "Zamora"),
        Provincia("50", "Zaragoza")
    )
}
```

---

## Variables para el spinner y su adaptador

Declaramos en el fragmento:

```kotlin
private lateinit var listaProvincias: Array<Provincia>
private lateinit var adaptador: ArrayAdapter<Provincia>
```

---

## Inicializar el spinner en `onViewCreated`

Finalmente, en `onViewCreated` cargamos la lista, creamos el adaptador y lo asignamos al spinner:

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // ...

    listaProvincias = generadorProvincias()
    adaptador = ArrayAdapter(requireActivity().applicationContext, android.R.layout.simple_spinner_item, listaProvincias)
    adaptador.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
    spinner.adapter = adaptador

    // ...
}
```

---

Con esto, tu spinner cargará la lista de provincias y mostrará sus nombres correctamente.


```kotlin
// 1. Declarar variable SharedPreferences
private lateinit var prefs: SharedPreferences

// 2. Inicializar en onViewCreated
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // ... ya tienes aquí inicializados spinner, adaptador, listaProvincias

    prefs = requireActivity().getSharedPreferences("Preferencias", MODE_PRIVATE)

    // Inicializar spinner y adaptador
    listaProvincias = generadorProvincias()
    adaptador = ArrayAdapter(requireActivity().applicationContext, android.R.layout.simple_spinner_item, listaProvincias)
    adaptador.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
    spinner.adapter = adaptador

    // Cargar valores guardados en SharedPreferences
    cargarPreferencias()
}

// 3. Método para cargar preferencias guardadas y actualizar UI
fun cargarPreferencias() {
    // Nombre guardado (o "Daniel" por defecto)
    val pref_nombre = prefs.getString("nombre", "Daniel")
    nAlu.setText(pref_nombre)

    // Id de la provincia guardada (o "30" = Murcia por defecto)
    val pref_spinner = prefs.getString("spinner", "30")

    // Buscar la posición en la lista donde coincida el id guardado
    val posicionSeleccionada = listaProvincias.indexOfFirst { it.id == pref_spinner }

    // Seleccionar esa posición en el spinner (si no encuentra devuelve -1, y no cambia selección)
    if (posicionSeleccionada != -1) {
        spinner.setSelection(posicionSeleccionada)
    }
}
```

---

### Explicación breve:

* `prefs` es la instancia de `SharedPreferences` con nombre `"Preferencias"`.
* En `cargarPreferencias()`, recuperamos el nombre guardado (o ponemos `"Daniel"` por defecto).
* Recuperamos el `id` de la provincia guardada (o `"30"` para Murcia).
* Buscamos en el array de provincias cuál tiene ese id y asignamos la selección en el spinner.
* Así el fragmento inicia mostrando los valores guardados o valores por defecto si es la primera ejecución.

---
Perfecto, aquí tienes cómo añadir el listener al botón y el método para guardar los datos en `SharedPreferences` dentro de tu fragmento:

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    
    // Enlaces a widgets (ya los tienes)
    nAlu = view.findViewById(R.id.etNumeroAlu)
    spinner = view.findViewById(R.id.spinner)
    guardar = view.findViewById(R.id.SPGuardar)
    
    // Inicializar SharedPreferences y spinner
    prefs = requireActivity().getSharedPreferences("Preferencias", MODE_PRIVATE)
    listaProvincias = generadorProvincias()
    adaptador = ArrayAdapter(requireActivity().applicationContext, android.R.layout.simple_spinner_item, listaProvincias)
    adaptador.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
    spinner.adapter = adaptador
    
    // Cargar valores guardados
    cargarPreferencias()
    
    // Listener para botón guardar
    guardar.setOnClickListener {
        guardarPreferencias()
    }
}

private fun guardarPreferencias() {
    val editor = prefs.edit()
    editor.putString("nombre", nAlu.text.toString())
    
    val provinciaSeleccionada = spinner.selectedItem as Provincia
    editor.putString("spinner", provinciaSeleccionada.id)
    
    editor.apply()
}
```

---
