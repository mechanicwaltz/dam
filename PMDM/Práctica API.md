# Practica API Libros

## `MainActivity`

Esta es la actividad principal de la aplicación. Hereda de `AppCompatActivity` y maneja la interfaz de usuario y la lógica principal.

#### Propiedades

- `recy`: Un `RecyclerView` que muestra una lista de autores.
- `adaptadorRecyclerView`: Una instancia de `AdaptadorAutores` que gestiona los datos y las vistas para el `RecyclerView`.
- `buscador`: Un `SearchView` que permite a los usuarios buscar autores.

#### Métodos

- **onCreateOptionsMenu**: Infla el menú de opciones desde `toolbar_principal.xml`.
  ```kotlin
  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
      menuInflater.inflate(R.menu.toolbar_principal, menu)
      return super.onCreateOptionsMenu(menu)
  }
  ```

- **onOptionsItemSelected**: Maneja la selección de elementos del menú. Abre la actividad `Favoritos` cuando se selecciona la opción de favoritos.
  ```kotlin
  override fun onOptionsItemSelected(item: MenuItem): Boolean {
      when(item.itemId){
          R.id.action_favoritos -> {
              val intent = Intent(this, Favoritos::class.java)
              startActivity(intent)
          }
      }
      return super.onOptionsItemSelected(item)
  }
  ```

- **onCreate**: Configura la actividad, infla el layout `activity_main`, configura la `Toolbar`, `SearchView` y `RecyclerView`. También establece un callback para guardar autores en la base de datos local.
  ```kotlin
  override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main)
      setSupportActionBar(findViewById(R.id.toolBarPrincipal))

      // Vincular vistas
      buscador = findViewById(R.id.svAutores)
      buscador.setOnQueryTextListener(this)
      recy = findViewById(R.id.rvAutores)
      recy.layoutManager = LinearLayoutManager(applicationContext, LinearLayoutManager.VERTICAL, false)
      recy.adapter = adaptadorRecyclerView

      adaptadorRecyclerView.setAdaptadorCallBack(object : AdaptadorAutores.AdaptadorCallBack {
          override fun onSaveAutor(aut: Autores) {
              lifecycleScope.launch(Dispatchers.IO) {
                  val database = AutoresBBDD.getInstance(applicationContext)
                  database.autoresDAO().insert(aut)
              }
          }
      })
  }
  ```

- **getRetrofit**: Configura y devuelve una instancia de `Retrofit` para realizar llamadas a la API.
  ```kotlin
  private fun getRetrofit(): Retrofit {
      return Retrofit.Builder()
          .baseUrl("https://openlibrary.org/")
          .addConverterFactory(GsonConverterFactory.create())
          .build()
  }
  ```

- **busquedaAutores**: Realiza una búsqueda de autores en la API de OpenLibrary y actualiza el `RecyclerView` con los resultados.
  ```kotlin
  private fun busquedaAutores(query: String) {
      lifecycleScope.launch(Dispatchers.IO) {
          val call = getRetrofit().create(APIservice::class.java).getAutores("/search/authors.json?q=$query")
          val autoresAPI = call.body()
          if (call.isSuccessful) {
              val autores = autoresAPI?.autores ?: emptyList()
              withContext(Dispatchers.Main) {
                  adaptadorRecyclerView.changelist(autores)
                  adaptadorRecyclerView.notifyDataSetChanged()
              }
          } else {
              Toast.makeText(applicationContext, "error API", Toast.LENGTH_SHORT).show()
          }
      }
  }
  ```

- **onQueryTextSubmit**: Maneja la acción de enviar una consulta en el `SearchView`, realiza la búsqueda y oculta el teclado.
  ```kotlin
  override fun onQueryTextSubmit(query: String?): Boolean {
      if (!query.isNullOrEmpty()) {
          busquedaAutores(query.lowercase())
      }
      val imm = getSystemService(INPUT_METHOD_SERVICE) as InputMethodManager
      imm.hideSoftInputFromWindow(this.buscador.windowToken, 0)
      return true
  }
  ```

- **onQueryTextChange**: Método requerido por la interfaz `SearchView.OnQueryTextListener`, pero no realiza ninguna acción.
  ```kotlin
  override fun onQueryTextChange(newText: String?): Boolean {
      return true
  }
  ```

### Resumen

- `MainActivity` maneja la interfaz principal y la lógica de búsqueda.
- `onCreateOptionsMenu` y `onOptionsItemSelected` gestionan el menú de opciones.
- `onCreate` configura los componentes de la interfaz de usuario y los vincula.
- `getRetrofit` configura el cliente de la API.
- `busquedaAutores` realiza la búsqueda en la API y actualiza la interfaz de usuario.
- `onQueryTextSubmit` maneja el envío de consultas de búsqueda.
- `onQueryTextChange` es un método requerido pero no realiza ninguna acción.

----
### `AdaptadorAutores`

Este adaptador se utiliza para manejar los datos y la vista de los elementos en el `RecyclerView`.

#### Propiedades

- `autores`: Una lista mutable de objetos `Autores` que contiene los datos a mostrar.
- `callback`: Una instancia de `AdaptadorCallBack` para manejar eventos de guardado de autores.

#### Métodos

- **onCreateViewHolder**: Infla el layout de cada elemento del `RecyclerView`.
- **onBindViewHolder**: Vincula los datos de un autor a la vista.
- **getItemCount**: Devuelve el número de elementos en la lista.
- **changelist**: Actualiza la lista de autores.
- **setAdaptadorCallBack**: Establece el callback para eventos de guardado.

```kotlin
class AdaptadorAutores : RecyclerView.Adapter<AdaptadorAutores.ViewHolder>() {
    private var autores: MutableList<Autores> = mutableListOf()
    private var callback: AdaptadorCallBack? = null

    interface AdaptadorCallBack {
        fun onSaveAutor(aut: Autores)
    }

    fun setAdaptadorCallBack(callback: AdaptadorCallBack) {
        this.callback = callback
    }

    fun changelist(autores: List<Autores>) {
        this.autores = autores.toMutableList()
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_autor, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val autor = autores[position]
        holder.bind(autor)
        holder.itemView.setOnClickListener {
            callback?.onSaveAutor(autor)
        }
    }

    override fun getItemCount(): Int {
        return autores.size
    }

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(autor: Autores) {
            // Vincular datos a las vistas
        }
    }
}
```

### `AdaptadorFavoritos`

Este adaptador es similar a `AdaptadorAutores` pero se utiliza específicamente para manejar autores favoritos.

#### Propiedades

- `favoritos`: Una lista mutable de objetos `Autores` que contiene los autores favoritos.
- `callback`: Una instancia de `AdaptadorCallBack` para manejar eventos de guardado de autores.

#### Métodos

- **onCreateViewHolder**: Infla el layout de cada elemento del `RecyclerView`.
- **onBindViewHolder**: Vincula los datos de un autor a la vista.
- **getItemCount**: Devuelve el número de elementos en la lista.
- **changelist**: Actualiza la lista de autores favoritos.
- **setAdaptadorCallBack**: Establece el callback para eventos de guardado.

```kotlin
class AdaptadorFavoritos : RecyclerView.Adapter<AdaptadorFavoritos.ViewHolder>() {
    private var favoritos: MutableList<Autores> = mutableListOf()
    private var callback: AdaptadorCallBack? = null

    interface AdaptadorCallBack {
        fun onSaveAutor(aut: Autores)
    }

    fun setAdaptadorCallBack(callback: AdaptadorCallBack) {
        this.callback = callback
    }

    fun changelist(favoritos: List<Autores>) {
        this.favoritos = favoritos.toMutableList()
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_autor, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val autor = favoritos[position]
        holder.bind(autor)
        holder.itemView.setOnClickListener {
            callback?.onSaveAutor(autor)
        }
    }

    override fun getItemCount(): Int {
        return favoritos.size
    }

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(autor: Autores) {
            // Vincular datos a las vistas
        }
    }
}
```

### `AuthorsResponse`

Clase de datos que representa la respuesta de la API de OpenLibrary. Contiene el número de resultados encontrados y una lista de autores.

```kotlin
data class AuthorsResponse(
    var numFound: Int,
    @SerializedName("docs") var autores: List<Autores>
)
```

### `Autores`

Clase de datos que representa a un autor. Contiene los atributos necesarios para almacenar la información de un autor.

```kotlin
data class Autores(
    val name: String,
    val birth_date: String?,
    val death_date: String?,
    val top_work: String?,
    val work_count: Int
)
```

### `AutoresBBDD`

Clase que maneja la base de datos local utilizando Room. Proporciona acceso a la base de datos y a los DAOs.

#### Propiedades

- `autoresDAO`: Un DAO para acceder a los datos de los autores.

#### Métodos

- **getInstance**: Devuelve una instancia de la base de datos.

```kotlin
@Database(entities = [Autores::class], version = 1)
abstract class AutoresBBDD : RoomDatabase() {
    abstract fun autoresDAO(): AutoresDAO

    companion object {
        @Volatile
        private var INSTANCE: AutoresBBDD? = null

        fun getInstance(context: Context): AutoresBBDD {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AutoresBBDD::class.java,
                    "autores_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

### `AutoresViewHolder`

Clase ViewHolder para vincular los datos de los autores a las vistas en el `RecyclerView`.

```kotlin
class AutoresViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    fun bind(autor: Autores) {
        // Vincular datos a las vistas
    }
}
```

### `Favoritos`

Actividad que muestra los autores guardados como favoritos. Se abre desde el menú de opciones en `MainActivity`.

#### Propiedades

- `recy`: Un `RecyclerView` que muestra la lista de autores favoritos.
- `adaptadorRecyclerView`: Una instancia de `AdaptadorAutores` para gestionar los datos y vistas.

#### Métodos

- **onCreate**: Configura la actividad, infla el layout `activity_favoritos`, y carga los autores favoritos desde la base de datos.

```kotlin
class Favoritos : AppCompatActivity() {
    private lateinit var recy: RecyclerView
    private val adaptadorRecyclerView = AdaptadorAutores()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_favoritos)
        setSupportActionBar(findViewById(R.id.toolBarFavoritos))

        recy = findViewById(R.id.rvFavoritos)
        recy.layoutManager = LinearLayoutManager(applicationContext, LinearLayoutManager.VERTICAL, false)
        recy.adapter = adaptadorRecyclerView

        lifecycleScope.launch(Dispatchers.IO) {
            val database = AutoresBBDD.getInstance(applicationContext)
            val autores = database.autoresDAO().getAll()
            withContext(Dispatchers.Main) {
                adaptadorRecyclerView.changelist(autores)
                adaptadorRecyclerView.notifyDataSetChanged()
            }
        }
    }
}
```

### `APIservice`

Interfaz que define los endpoints de la API de OpenLibrary. Utiliza Retrofit para realizar las llamadas a la API.

```kotlin
interface APIservice {
    @GET
    suspend fun getAutores(@Url url: String): Response<AuthorsResponse>
}
```

## INTERFACES:

### `AutoresDAO`

Objeto de Acceso a Datos (DAO) para acceder a los datos de los autores en la base de datos local.

#### Métodos

- **insert**: Inserta un autor en la base de datos.
- **getAll**: Recupera todos los autores de la base de datos.

```kotlin
@Dao
interface AutoresDAO {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(autor: Autores)

    @Query("SELECT * FROM autores")
    suspend fun getAll(): List<Autores>
}
```
