
## ⭐ Favoritos

En esta sección implementamos la funcionalidad de **guardar municipios como favoritos**, mostrándolos posteriormente en una lista con opciones de **eliminar** y **ver detalles**.

---

### 🏗️ Estructura General

* Nueva actividad: `FavoritosActivity`
* Layout: `activity_favoritos.xml`
* Layout de fila: `fila_favoritos.xml`
* Adaptador personalizado: `adaptadorFavoritos`
* Toolbar con menú: `menu_fav.xml`
* ViewHolder: `FavoritosViewHolder`

---

## 🎯 Objetivo

1. Mostrar una lista de municipios favoritos.
2. Permitir eliminar un municipio con un botón en la `Toolbar`.
3. Lanzar `DetalleMunicipioActivity` al pulsar sobre un item.

---

## 🧩 Layout: `activity_favoritos.xml`

```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Mis favoritos"
    android:textAlignment="center"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent" />

<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerViewFavoritos"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginHorizontal="8dp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/textView" />
```

---

## 📄 Layout de fila: `fila_favoritos.xml`

```xml
<androidx.cardview.widget.CardView
    android:id="@+id/cvF"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="16dp"
    app:cardBackgroundColor="@color/favorito"
    app:cardCornerRadius="16dp"
    app:cardElevation="4dp">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/tbCard"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <TextView
            android:id="@+id/filaID"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="@string/CODIGOINE_municipio"
            android:textSize="20sp"
            android:textStyle="bold"
            app:layout_constraintTop_toBottomOf="@id/tbCard" />

        <TextView
            android:id="@+id/filaMunicipio"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="@string/nombre_municipio"
            android:textSize="30sp"
            android:textStyle="bold"
            app:layout_constraintTop_toBottomOf="@id/filaID" />

    </androidx.constraintlayout.widget.ConstraintLayout>
</androidx.cardview.widget.CardView>
```

---

## 🍔 Menú de Toolbar: `menu_fav.xml`

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/action_borrar"
        android:title="borrar"
        android:icon="@drawable/eliminar"
        app:showAsAction="ifRoom" />
</menu>
```

---

## 🔄 ViewHolder: `FavoritosViewHolder.kt`

```kotlin
class FavoritosViewHolder(view: View) : RecyclerView.ViewHolder(view) {
    var id: TextView = view.findViewById(R.id.filaID)
    var nombre: TextView = view.findViewById(R.id.filaMunicipio)
    var menucard: Toolbar = view.findViewById(R.id.tbCard)
    var cv: CardView = view.findViewById(R.id.cvF)
}
```

---

## 🧠 Adaptador Básico: `adaptadorFavoritos.kt`

```kotlin
class adaptadorFavoritos : RecyclerView.Adapter<FavoritosViewHolder>() {

    private var favs: List<Municipio> = emptyList()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): FavoritosViewHolder {
        val vista = LayoutInflater.from(parent.context)
            .inflate(R.layout.fila_favoritos, parent, false)
        val viewHolder = FavoritosViewHolder(vista)
        viewHolder.menucard.inflateMenu(R.menu.menu_fav)
        return viewHolder
    }

    override fun getItemCount(): Int = favs.size

    override fun onBindViewHolder(holder: FavoritosViewHolder, position: Int) {
        val item = favs[position]
        holder.id.text = item.CODIGOINE
        holder.nombre.text = item.NOMBRE

        // TODO: Añadir listeners para borrar y lanzar actividad
    }

    fun actualizarLista(lista: List<Municipio>) {
        favs = lista
        notifyDataSetChanged()
    }
}
```

Perfecto. Aquí tienes la **configuración del adaptador y RecyclerView en `FavoritosActivity.kt`** documentada en **Markdown**, ideal para mantener organizada tu documentación del proyecto:

---

## 🔧 Configurar Adaptador y RecyclerView en `FavoritosActivity`

En esta sección se configura el `RecyclerView` para mostrar los municipios favoritos guardados en base de datos.

---

### ✅ Variables necesarias

Declaramos las variables del `RecyclerView` y su adaptador:

```kotlin
private lateinit var rvFavoritos: RecyclerView
val adaptadorRVFavoritos = adaptadorFavoritos()
```

---

### 🔁 Configuración en `onCreate`

En el método `onCreate`, se bindea el `RecyclerView` del layout y se configura el adaptador junto con el layout manager:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_favoritos)

    rvFavoritos = findViewById(R.id.recyclerViewFavoritos)

    rvFavoritos.layoutManager = LinearLayoutManager(
        this,
        LinearLayoutManager.VERTICAL,
        false
    )

    rvFavoritos.adapter = adaptadorRVFavoritos
}
```

---

## 🗃️ Configurar Base de Datos con Room (BBDD)

Room es una librería de persistencia que simplifica el acceso a la base de datos SQLite mediante una capa de abstracción.

---

### 🧱 Paso 1: Añadir dependencias en `build.gradle`

```kotlin
val room_version = "2.6.1"

implementation("androidx.room:room-runtime:$room_version")
annotationProcessor("androidx.room:room-compiler:$room_version")
kapt("androidx.room:room-compiler:$room_version")
```

**Plugins requeridos:**

```kotlin
id("kotlin-kapt")
```

---

### 📦 Estructura de Room

Room se compone de 3 elementos principales:

* **Entity**: Representa una tabla.
* **DAO (Data Access Object)**: Define las operaciones sobre la tabla.
* **Database**: Clase principal que agrupa las entidades y DAOs.

> Todas las clases se ubican en un paquete llamado `sqlite`.

---

## 1️⃣ Entity: `Municipio`

```kotlin
@Entity
data class Municipio(
    @PrimaryKey var CODIGOINE: String,
    var NOMBRE: String
)
```

* `@Entity`: Marca la clase como una tabla.
* `@PrimaryKey`: Define la clave primaria (no autogenerada en este caso).

---

## 2️⃣ DAO: `FavoritosDAO`

```kotlin
@Dao
interface FavoritosDAO {
    @Query("SELECT * FROM Municipio")
    fun selectAll(): List<Municipio>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun insert(fav: Municipio): Long

    @Delete
    fun delete(fav: Municipio)
}
```

* `@Insert`: Inserta un municipio (reemplaza si ya existe).
* `@Delete`: Elimina un municipio por su clave.
* `@Query`: Consulta para obtener todos los favoritos.

---

## 3️⃣ Database: `FavoritosBBDD`

```kotlin
@Database(entities = [Municipio::class], version = 1, exportSchema = false)
abstract class FavoritosBBDD : RoomDatabase() {
    abstract fun favoritosDAO(): FavoritosDAO

    companion object DatabaseBuilder {
        private var INSTANCE: FavoritosBBDD? = null

        fun getInstance(context: Context): FavoritosBBDD {
            if (INSTANCE == null) synchronized(Municipio::class) {
                INSTANCE = buildRoomDB(context)
            }
            return INSTANCE!!
        }

        private fun buildRoomDB(context: Context) =
            Room.databaseBuilder(
                context.applicationContext,
                FavoritosBBDD::class.java,
                "favoritos.db"
            ).fallbackToDestructiveMigration().build()
    }
}
```

* `@Database`: Indica las entidades y versión.
* `Room.databaseBuilder(...)`: Construye o accede a la instancia singleton.
* `.fallbackToDestructiveMigration()`: Borra y recrea si hay cambios estructurales.

---




## 🏗️ Cargar datos de la BBDD en FavoritosActivity

En el método `onCreate` o donde quieras cargar los datos, usa una coroutine para acceder a la base de datos en hilo de fondo (`Dispatchers.IO`) y luego actualizar la UI en el hilo principal (`Dispatchers.Main`):

```kotlin
lifecycleScope.launch(Dispatchers.IO) {
    val database = applicationContext?.let { FavoritosBBDD.getInstance(it) }
    val datos = database?.favoritosDAO()?.selectAll() as MutableList<Municipio>
    
    withContext(Dispatchers.Main) {
        adaptadorRVFavoritos.changeList(datos)
        adaptadorRVFavoritos.notifyDataSetChanged()
    }
}
```

---

## ✏️ Método `changeList` en el adaptador para actualizar la lista

Añade este método en tu adaptador `adaptadorFavoritos`:

```kotlin
fun changeList(datos: MutableList<Municipio>) {
    favs = datos
}
```

---

Con esto:

* Se obtiene la lista completa de municipios guardados en la base de datos.
* Se actualiza la lista del adaptador.
* Se notifica el cambio para refrescar el RecyclerView.

---

Perfecto, aquí tienes el resumen y el código para añadir la funcionalidad de **guardar un municipio en la base de datos desde DetalleActivity** y lanzar la actividad Favoritos desde MainActivity.

---

## 🚀 Lanzar FavoritosActivity desde MainActivity (opción menú)

Solo tienes que agregar en tu `onOptionsItemSelected` la opción para lanzar FavoritosActivity cuando se pulse el ítem `action_fav`:

```kotlin
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return when (item.itemId) {
        R.id.action_fav -> {
            startActivity(Intent(this, FavoritosActivity::class.java))
            true
        }
        else -> super.onOptionsItemSelected(item)
    }
}
```

---

## ➕ Añadir municipio a favoritos desde DetalleActivity

1. Declara la variable para el LinearLayout que contiene el botón o zona para añadir favoritos:

```kotlin
private lateinit var linearLayout: LinearLayout
```

2. En `onCreate`, haz el bind con el id correspondiente (ajusta `R.id.ll` a tu id real):

```kotlin
linearLayout = findViewById(R.id.ll)
```

3. Pon el listener para insertar el municipio en la base de datos usando coroutine para no bloquear UI:

```kotlin
linearLayout.setOnClickListener {
    lifecycleScope.launch(Dispatchers.IO) {
        val database = FavoritosBBDD.getInstance(applicationContext)
        database.favoritosDAO().insert(
            Municipio(municipioCodigo.toString(), municipioNombre.toString())
        )
        withContext(Dispatchers.Main) {
            Toast.makeText(applicationContext, "Guardado", Toast.LENGTH_LONG).show()
        }
    }
}
```

---

### Notas

* `municipioCodigo` y `municipioNombre` se obtienen del intent o de donde tengas guardados los datos actuales del municipio en `DetalleActivity`.
* La inserción usará el DAO que ya definiste para `insert`.
* El `Toast` confirmará al usuario que se ha guardado correctamente.

---


Claro, aquí tienes todo el código y explicación en markdown para implementar la eliminación y el lanzamiento de detalle desde el adaptador y la actividad FavoritosActivity:

---

# Eliminar Favorito y Lanzar Detalle desde FavoritosActivity

### 1. Añadir callback en el adaptador

En tu adaptador `adaptadorFavoritos`:

```kotlin
class adaptadorFavoritos : RecyclerView.Adapter<FavoritosViewHolder>() {

    private var favs: List<Municipio> = emptyList()

    private lateinit var listener: AdaptadorCallback

    interface AdaptadorCallback {
        fun lanzarAct(fav: Municipio)
        fun eliminarfav(fav: Municipio)
    }

    fun setAdaptadorCallback(listener: AdaptadorCallback) {
        this.listener = listener
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): FavoritosViewHolder {
        val vista: View =
            LayoutInflater.from(parent.context).inflate(R.layout.fila_favoritos, parent, false)
        val viewHolder = FavoritosViewHolder(vista)
        viewHolder.menucard.inflateMenu(R.menu.menu_fav)
        return viewHolder
    }

    override fun getItemCount(): Int = favs.size

    override fun onBindViewHolder(holder: FavoritosViewHolder, position: Int) {
        val item = favs[position]
        holder.id.text = item.CODIGOINE
        holder.nombre.text = item.NOMBRE

        // Lanzar actividad detalle al pulsar el CardView
        holder.cv.setOnClickListener {
            listener.lanzarAct(item)
        }

        // Borrar favorito desde el menú de la Toolbar
        holder.menucard.setOnMenuItemClickListener {
            when (it.itemId) {
                R.id.action_borrar -> {
                    listener.eliminarfav(item)
                    true
                }
                else -> false
            }
        }
    }

    fun changeList(datos: MutableList<Municipio>) {
        favs = datos
    }
}
```

---

### 2. Implementar los callbacks en FavoritosActivity

En `FavoritosActivity`, después de instanciar el adaptador, pon el callback:

```kotlin
adaptadorRVFavoritos.setAdaptadorCallback(object : adaptadorFavoritos.AdaptadorCallback {
    override fun lanzarAct(fav: Municipio) {
        val intent = Intent(applicationContext, DetalleMunicipioActivity::class.java)
        intent.putExtra("municipio_codigo", fav.CODIGOINE)
        intent.putExtra("municipio_nombre", fav.NOMBRE)
        startActivity(intent)
    }

    override fun eliminarfav(fav: Municipio) {
        lifecycleScope.launch(Dispatchers.IO) {
            val database = applicationContext?.let { FavoritosBBDD.getInstance(it) }
            database?.favoritosDAO()?.delete(fav)
            val datos = database?.favoritosDAO()?.selectAll() as MutableList<Municipio>
            withContext(Dispatchers.Main) {
                adaptadorRVFavoritos.changeList(datos)
                adaptadorRVFavoritos.notifyDataSetChanged()
            }
        }
    }
})
```

---

### 3. Explicación

* **Lanzar detalle:** cuando se pulsa en el CardView, se usa el callback para abrir `DetalleMunicipioActivity` enviando código y nombre.
* **Eliminar favorito:** el menú contextual en la toolbar del cardview tiene el icono de borrar. Al pulsar, llama al callback que elimina el item en la BBDD y refresca el listado.

---


