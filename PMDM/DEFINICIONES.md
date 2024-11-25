## Anatomía de una Aplicación Android 
### 1. Activities
Android utiliza el concepto de “pantalla” o “vista” donde los usuarios realmente interactúan con su programa. Android llama a estas activities, y ya sea que desee que sus usuarios lean texto, vean un video, ingresen datos, hagan una llamada telefónica, jueguen un juego o lo que sea, sus usuarios lo harán interactuando con una o varias pantallas de actividad o diseños que que nosotros implementemos.

Las actividades definen lo que hace la aplicación. Cada aplicación de Android incluye una o más actividades. Una actividad es una clase especial, generalmente escrita en Kotlin, que controla el comportamiento de la aplicación y decide cómo responder al usuario. Si la aplicación incluye un botón, por ejemplo, agrega código a la actividad que indica qué debe hacer el botón cuando el usuario lo toca o hace clic en él.

### 2. Fragmentos

Una actividad, como se describió anteriormente, generalmente representa una única pantalla de interfaz de usuario dentro de una aplicación. Una opción es construir la actividad utilizando un único diseño de interfaz de usuario y un archivo de clase de actividad correspondiente. Sin embargo, una mejor alternativa es dividir la actividad en diferentes secciones. Cada una de estas secciones se denomina fragmento, cada una de las cuales consta de una parte del diseño de interfaz de usuario y un archivo de clase correspondiente (declarado como una subclase de la clase Fragmento de Android). En este escenario, una actividad simplemente se convierte en un contenedor en el que se incrustan uno o más fragmentos.

De hecho, los fragmentos proporcionan una alternativa eficiente a tener cada pantalla de interfaz de usuario representada por una actividad separada. En cambio, una aplicación puede constar de una única actividad que cambia entre diferentes fragmentos, cada uno
representando una pantalla de aplicación diferente.

### 3. Intents

Los intents son el sistema de mensajería interna de Android, que permite pasar información entre aplicaciones y hacia y desde el entorno de Android. De esta manera, su aplicación puede desencadenar acciones y compartir datos con Android y otras aplicaciones, y también puede escuchar los eventos que están sucediendo y tomar medidas cuando sea apropiado.

El sistema operativo Android ya tiene una gama muy amplia de intents con los que su aplicación puede interactuar y, como desarrollador, también puede definir y desarrollar sus propios intents para sus propios usos.

Todos los componentes (aplicaciones y pantallas) del dispositivo Android están aisladas. La única manera de comunicarse entre ellas es a través de intents.

Android admite intents explícitas e implícitas. Una aplicación puede definir el componente de destino directamente en la intent ( intents explícitas) o pedirle al sistema Android que evalúe los componentes registrados en función de los datos de la intención ( intents implícitas).

### 4. Services. 
Los servicios son un concepto común en gran parte del mundo de la informática y, si bien existen ligeras diferencias al tratarlos en Android, muchos de los conceptos comunes aún se aplican. Los servicios son aplicaciones que normalmente no tienen interfaz de usuario y, en cambio, se ejecutan en segundo plano. Los servicios ofrecen una variedad de características, capacidades o comportamientos que normalmente son demandados por múltiples aplicaciones. Los servicios suelen ser de larga duración y funcionan en segundo plano ayudando a su aplicación y a otras según sea necesario.

### 5. Content Providers
Las aplicaciones nativas de Android incluyen una serie de proveedores de contenido estándar que permiten a las aplicaciones acceder a datos como contactos y archivos multimedia

También puede crear y compartir sus propios proveedores de contenido, lo que facilita el intercambio de datos y la interacción con otras aplicaciones.

Claro, aquí tienes un resumen detallado de los elementos más importantes en el desarrollo de una aplicación Android con Kotlin, junto con ejemplos para cada uno:

---

### 1. **AndroidManifest.xml**
El archivo `AndroidManifest.xml` es esencial para configurar la aplicación. Define actividades, permisos, servicios, y más. Es el punto de entrada principal donde se registra toda la información sobre los componentes de la app.

#### Ejemplo básico:
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <!-- Define el nombre del paquete de la app. Es único para cada app. -->

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/Theme.MyApp">
        <!-- Configura la aplicación en general, como su icono, nombre y tema. -->

        <activity android:name=".MainActivity">
            <!-- Declara la actividad principal de la app. -->

            <intent-filter>
                <!-- Define cómo otros sistemas o apps pueden interactuar con esta actividad. -->

                <action android:name="android.intent.action.MAIN" />
                <!-- Especifica que esta actividad es el punto de entrada principal. -->

                <category android:name="android.intent.category.LAUNCHER" />
                <!-- Marca esta actividad para aparecer en el lanzador (inicio del dispositivo). -->
            </intent-filter>
        </activity>
    </application>
</manifest>
```
- **Puntos clave**: 
  - Declarar actividades con `<activity>`.
  - Establecer el punto de entrada con `<intent-filter>`.

---

### 2. **Intents**
Los Intents permiten comunicar componentes dentro de la app o incluso entre aplicaciones. Hay dos tipos: **Explícitos** (para actividades específicas) e **Implícitos** (para acciones generales como abrir un navegador).

#### Ejemplo de Intent explícito:
```kotlin
val intent = Intent(this, SecondActivity::class.java)

```
· Crea un Intent para iniciar `SecondActivity`, indicando que quieres ir específicamente a esa actividad.

```kotlin
intent.putExtra("EXTRA_MESSAGE", "Hola, Kotlin!")
```
· Agrega un dato extra ("Hola, Kotlin!") que `SecondActivity` puede recuperar.

```kotlin
startActivity(intent)
```
· Inicia la nueva actividad usando el intent.


#### Ejemplo de Intent implícito:
```kotlin
val intent = Intent(Intent.ACTION_VIEW).apply {
    data = Uri.parse("https://www.google.com")
}
startActivity(intent)
```
· Este código usa un Intent implícito para abrir el navegador web. Intent.ACTION_VIEW indica que quieres ver algo, y Uri.parse pasa la URL que se debe abrir.

---

### 3. **Fragmentos**
Los fragmentos son componentes modulares que representan partes de la interfaz. Son útiles para crear diseños dinámicos y reutilizables.

#### Crear un fragmento:
```kotlin
class MyFragment : Fragment(R.layout.fragment_my) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // Lógica del fragmento
    }
}
```

· `Fragment(R.layout.fragment_my)`: Asocia un archivo de diseño (fragment_my.xml) al fragmento.
· `onViewCreated`: Se ejecuta después de que la vista del fragmento se haya creado. Aquí colocas la lógica del fragmento, como manejar clics o actualizar textos.


#### Agregar un fragmento a una actividad:
```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.fragment_container, MyFragment())
    .commit()
```

· supportFragmentManager: Maneja los fragmentos de la actividad.
· replace: Reemplaza cualquier contenido previo en el contenedor (R.id.fragment_container) con MyFragment.
· commit: Aplica los cambios.


---

### 4. **Layouts**
Un layout define la estructura visual de la interfaz. Los más comunes son `LinearLayout`, `ConstraintLayout`, y `RelativeLayout`.

#### Ejemplo con ConstraintLayout:
```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
<!-- Un contenedor que organiza vistas usando restricciones. -->


    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Hola, mundo!"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>
        <!-- Este TextView está centrado horizontal y verticalmente en el contenedor. -->

</androidx.constraintlayout.widget.ConstraintLayout>
```

---

### 5. **ViewBinding**
ViewBinding facilita acceder a vistas sin necesidad de usar `findViewById`.

#### Ejemplo de uso:
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    // Crea una propiedad para el objeto ViewBinding asociado al diseño de la actividad.


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        // Inicializa el objeto ViewBinding inflando el diseño.

        setContentView(binding.root)
        // Asocia el diseño raíz de ViewBinding con la actividad.


        binding.textView.text = "Texto actualizado con ViewBinding"
        // Modifica directamente una vista usando el objeto binding.

    }
}
```

---

### 6. **RecyclerView**
Se utiliza para listas dinámicas y de gran tamaño. Es más eficiente que ListView.

#### Pasos básicos:
1. **Crear el adaptador:**
```kotlin
class MyAdapter(private val items: List<String>) : RecyclerView.Adapter<MyAdapter.ViewHolder>() {

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView.findViewById(R.id.textView)
        // Enlaza el TextView del diseño de cada ítem.
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_view, parent, false)
        // Infla el diseño de cada ítem y lo asocia al ViewHolder.
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position]
        // Asigna el contenido a cada ítem basado en su posición.
    }

    override fun getItemCount() = items.size
    // Devuelve el número total de elementos en la lista.
}

```

2. **Configurar RecyclerView en la actividad:**
```kotlin
val recyclerView: RecyclerView = findViewById(R.id.recyclerView)
recyclerView.layoutManager = LinearLayoutManager(this)
recyclerView.adapter = MyAdapter(listOf("Elemento 1", "Elemento 2", "Elemento 3"))
```

---

### 7. **LiveData y ViewModel**
Se usan para gestionar datos de manera reactiva y separar la lógica de la UI.

#### Ejemplo de ViewModel:
```kotlin
class MyViewModel : ViewModel() {
    private val _text = MutableLiveData("Texto inicial")
    val text: LiveData<String> get() = _text

    fun updateText(newText: String) {
        _text.value = newText
    }
}
```

#### Vincular en la actividad:
```kotlin
val viewModel: MyViewModel by viewModels()
viewModel.text.observe(this, { newText ->
    binding.textView.text = newText
})
```

---

### 8. **Navigation Component**
Facilita la navegación entre pantallas de forma declarativa.

#### Configurar NavGraph:
```xml
<fragment
    android:id="@+id/firstFragment"
    android:name="com.example.FirstFragment"
    android:label="First Fragment" />
<fragment
    android:id="@+id/secondFragment"
    android:name="com.example.SecondFragment"
    android:label="Second Fragment">
    <action
        android:id="@+id/action_to_secondFragment"
        app:destination="@id/secondFragment" />
</fragment>
```

#### Navegar entre fragmentos:
```kotlin
findNavController().navigate(R.id.action_to_secondFragment)
```
