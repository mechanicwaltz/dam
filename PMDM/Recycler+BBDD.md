


# Práctica guiada: RecyclerView + BBDD

La finalidad de esta práctica es implementar un RecyclerView con una base de datos local SQLite.

El uso de bases de datos tiene unas peculiaridades a la hora de actualizar los datos, por lo que implementarás corrutinas y un callback.

La aplicación no es más que una agenda de contactos en la que guardamos un nombre y correo electrónico. Añadiremos la función de agregar y eliminar un contacto.

---

## Layout principal

Lo primero que realizaremos será nuestro Layout principal, el cual consta de un `TextView`, dos `EditText`, un `Button` y el `RecyclerView`:

```xml
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textview"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:text="Agenda Correos"
        android:textSize="24sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/editTnombre"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="50dp"
        android:hint="Introduce tu nombre"
        android:padding="10dp"
        app:layout_constraintTop_toBottomOf="@id/textview" />

    <EditText
        android:id="@+id/editTemail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:hint="Introduce tu correo"
        android:inputType="textEmailAddress"
        android:padding="10dp"
        app:layout_constraintTop_toBottomOf="@+id/editTnombre" />

    <Button
        android:id="@+id/butAgregar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:hint="Agregar"
        android:padding="10dp"
        app:layout_constraintTop_toBottomOf="@+id/editTemail" />

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerViewPeliculas"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginHorizontal="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toBottomOf="@id/butAgregar" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

---

## Fila de usuarios

También crearemos nuestro layout `fila_usuarios`:

```xml
<androidx.cardview.widget.CardView
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/card_view"
    android:layout_gravity="center"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardUseCompatPadding="true"
    app:cardCornerRadius="4dp"
    android:layout_margin="8dp">

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/tbCard"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:theme="@style/ThemeOverlay.AppCompat.ActionBar" />

    <LinearLayout
        android:layout_width="match_parent"
        android:orientation="vertical"
        android:layout_height="wrap_content">

        <TextView
            android:id="@+id/filaNombre"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginTop="8dp"
            android:text="Nombre"
            android:textSize="30sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/filaEmail"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginTop="8dp"
            android:textSize="15sp"
            android:text="correo" />
    </LinearLayout>
</androidx.cardview.widget.CardView>
```

---

## Adaptador de usuarios

Nuestro adaptador será una versión básica:

```kotlin
class AdaptadorUsuarios : RecyclerView.Adapter<ViewHolderUsuario>() {
    private var usuarios: MutableList<Usuario> = mutableListOf()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolderUsuario {
        val vista = LayoutInflater.from(parent.context)
            .inflate(R.layout.fila_usuarios, parent, false)
        return ViewHolderUsuario(vista)
    }

    override fun getItemCount() = usuarios.size

    override fun onBindViewHolder(holder: ViewHolderUsuario, position: Int) {
        val usuario = usuarios[position]
        holder.textViewNombre.text = usuario.nombre
        holder.textViewMail.text = usuario.correo
    }
}
```

---

## ViewHolder

En el `ViewHolder` referenciamos todos los componentes:

```kotlin
class ViewHolderUsuario(itemView: View) : RecyclerView.ViewHolder(itemView) {
    val textViewNombre: TextView = itemView.findViewById(R.id.filaNombre)
    val textViewMail: TextView = itemView.findViewById(R.id.filaEmail)
    val menucard: Toolbar = itemView.findViewById(R.id.tbCard)
    val card: CardView = itemView.findViewById(R.id.card_view)
}
```

---

## Clase Usuario

Nuestra clase `Usuario` será compatible con Room:

```kotlin
@Entity(tableName = "users")
data class Usuario(
    @ColumnInfo(name = "name") var nombre: String,
    @ColumnInfo(name = "email") var correo: String
) {
    @PrimaryKey(autoGenerate = true) var id: Int = 0
}
```

---

## DAO básico

Definimos las operaciones básicas en el `DAO`:

```kotlin
@Dao
interface UsuarioDAO {
    @Query("SELECT * FROM users")
    fun selectAll(): List<Usuario>

    @Insert
    fun insert(usuario: Usuario): Long

    @Delete
    fun delete(usuario: Usuario)
}
```

---

## UsuariosBBDD

Creamos la base de datos con Room:

```kotlin
@Database(entities = [Usuario::class], version = 1, exportSchema = false)
abstract class UsuariosBBDD : RoomDatabase() {
    abstract fun usuarioDAO(): UsuarioDAO

    companion object {
        private var INSTANCE: UsuariosBBDD? = null

        fun getInstance(context: Context): UsuariosBBDD {
            if (INSTANCE == null) synchronized(Usuario::class) {
                INSTANCE = buildRoomDB(context)
            }
            return INSTANCE!!
        }

        private fun buildRoomDB(context: Context) =
            Room.databaseBuilder(
                context.applicationContext, 
                UsuariosBBDD::class.java, 
                "usuarios.db"
            ).build()
    }
}
```

---

## Referencia de componentes en MainActivity

Referenciamos los componentes necesarios:

```kotlin
private lateinit var nombre: EditText
private lateinit var mail: EditText
private lateinit var agregar: Button

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    nombre = findViewById(R.id.editTnombre)
    mail = findViewById(R.id.editTemail)
    agregar = findViewById(R.id.butAgregar)
}
```

---

## Asociación del RecyclerView al adaptador

Asociamos el RecyclerView al adaptador y cargamos datos desde la base de datos:

```kotlin
val recyclerViewPeliculas: RecyclerView = findViewById(R.id.recyclerViewPeliculas)
val adaptadorRecyclerView = AdaptadorUsuarios()
recyclerViewPeliculas.layoutManager = LinearLayoutManager(
    applicationContext, LinearLayoutManager.VERTICAL, false
)
recyclerViewPeliculas.adapter = adaptadorRecyclerView

lifecycleScope.launch(Dispatchers.IO) {
    val database = UsuariosBBDD.getInstance(applicationContext)
    val datosUsuarios = database.usuarioDAO().selectAll().toMutableList()
    withContext(Dispatchers.Main) {
        adaptadorRecyclerView.changeList(datosUsuarios)
    }
}
```

---

## Método changeList

El método `changeList` actualiza los datos del adaptador:

```kotlin
fun changeList(usuarios: MutableList<Usuario>) {
    this.usuarios = usuarios
    notifyDataSetChanged()
}
```

---

## Funcionalidad del botón "Agregar"

Agregamos funcionalidad al botón para insertar un usuario:

```kotlin
agregar.setOnClickListener {
    lifecycleScope.launch(Dispatchers.IO) {
        val database = UsuariosBBDD.getInstance(applicationContext)
        database.usuarioDAO().insert(
            Usuario(nombre.text.toString(), mail.text.toString())
        )
        val nuevaLista = database.usuarioDAO().selectAll()
        withContext(Dispatchers.Main) {
            adaptadorRecyclerView.changeList(nuevaLista.toMutableList())
        }
    }

    val inputMethodManager = getSystemService(INPUT_METHOD_SERVICE) as InputMethodManager
    inputMethodManager.hideSoftInputFromWindow(agregar.windowToken, 0)
}
```

---

## Funcionalidad de borrado

Para borrar un contacto utilizaremos un menú en el CardView:

### Layout del menú (`menu_fila.xml`)

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/action_borrar"
        android:title="Borrar"
        android:orderInCategory="100"
        app:showAsAction="never" />
</menu>
```

### Inflar el menú en el adaptador

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolderUsuario {
    val vista = LayoutInflater.from(parent.context).inflate(R.layout.fila_usuarios, parent, false)
    val viewHolder = ViewHolderUsuario(vista)
    viewHolder.menucard.inflateMenu(R.menu.menu_fila)
    return viewHolder
}
```

### Implementar el listener para borrar

```kotlin
override fun onBindViewHolder(holder: ViewHolderUsuario, position: Int) {
    val usuario = usuarios[position]
    holder.textViewNombre.text = usuario.nombre
    holder.textViewMail.text = usuario.correo

    holder.menucard.setOnMenuItemClickListener {
        when (it.itemId) {
            R.id.action_borrar -> {
                listener.onDeleteUsuario(usuario)
                true
            }
            else -> true
        }
    }
}
```

---

## Uso de callback para el borrado

Añadimos una interfaz en el adaptador:

```kotlin
private lateinit var listener: AdaptadorCallback

interface AdaptadorCallback {
    fun onDeleteUsuario(usuario: Usuario)
}

fun setAdaptadorCallback(listener: AdaptadorCallback) {
    this.listener = listener
}
```

Implementamos el callback en `MainActivity`:

```kotlin
adaptadorRecyclerView.setAdaptadorCallback(object : AdaptadorUsuarios.AdaptadorCallback {
    override fun onDeleteUsuario(usuario: Usuario) {
        lifecycleScope.launch(Dispatchers.IO) {
            val database = UsuariosBBDD.getInstance(applicationContext)
            database.usuarioDAO().delete(usuario)
            val datos = database.usuarioDAO().selectAll().toMutableList()
            withContext(Dispatchers.Main) {
                adaptadorRecyclerView.changeList(datos)
            }
        }
    }
})
```

---

## Mejora en el rendimiento de eliminación

Para evitar recargar todo el RecyclerView, usamos `notifyItemRemoved`:

```kotlin
fun removeUsuario(usuario: Usuario) {
    val position = usuarios.indexOf(usuario)
    if (position != -1) {
        usuarios.removeAt(position)
        notifyItemRemoved(position)
    }
}
```

Actualizamos el callback para usar `removeUsuario`:

```kotlin
adaptadorRecyclerView.setAdaptadorCallback(object : AdaptadorUsuarios.AdaptadorCallback {
    override fun onDeleteUsuario(usuario: Usuario) {
        lifecycleScope.launch(Dispatchers.IO) {
            val database = UsuariosBBDD.getInstance(applicationContext)
            database.usuarioDAO().delete(usuario)
            withContext(Dispatchers.Main) {
                adaptadorRecyclerView.removeUsuario(usuario)
            }
        }
    }
})
```

---
