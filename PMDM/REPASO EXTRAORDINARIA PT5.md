# API - Buscador

Incorporar el código para que este fragmento realice una llamada a la API del tiempo y devuelva los municipios.

## Layout

Lo primero que realizaremos será crear nuestro layout del buscador. Para ello tenemos que modificar el layout del fragmento (`fragmento_buscador.xml`).
![image](https://github.com/user-attachments/assets/1b0f8a70-a0d4-44d7-a499-2e6add182cc0)


Como podéis ver, este layout solo contiene un `SearchView` y un `RecyclerView`.

He utilizado los siguientes IDs:

- **SearchView:** `svMunicipios`
- **RecyclerView:** `rvMunicipios`

---

A continuación crearemos un `fila_municipios.xml` para el layout de cada uno de los elementos del RecyclerView. Este layout es muy sencillo ya que solo contiene un `CardView` con dos `TextView`.

Identificadores utilizados para los TextView:
![image](https://github.com/user-attachments/assets/8c68629c-7c38-49ab-92d0-9dc88cfa3453)


- `filaMunicipio`
- `filaID`



---

# API - Buscador

Incorporar el código para que este fragmento realice una llamada a la API del tiempo y devuelva los municipios.

---

## Adaptador

Una vez creados los dos layouts, vamos a crear el adaptador para nuestro `RecyclerView`.

### ViewHolder

El `ViewHolder` es muy sencillo ya que solo tiene dos elementos. Lo he creado dentro de un paquete `adaptadores` para mantener los archivos organizados.

```kotlin
class MunicipioViewHolder(view: View) : RecyclerView.ViewHolder(view) {
    var codigo: TextView
    var nombre: TextView

    init {
        codigo = view.findViewById(R.id.filaID)
        nombre = view.findViewById(R.id.filaMunicipio)
    }
}
````

### Adaptador

El siguiente elemento a crear es el propio adaptador. Parte del código se facilitaba. Solo hay que indicar qué elementos de cada ítem de `Municipios` corresponden a cada elemento de nuestro `fila_municipios.xml`. Esto lo hacemos en `onBindViewHolder`:

```kotlin
val item = listaMunicipiosFiltrada[position]
holder.codigo.text = item.CODIGOINE
holder.nombre.text = item.NOMBRE
```

# Configuración API

Dado que nuestra app obtendrá los datos desde internet, necesitamos añadir el permiso en el `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
````

---

## Añadir librerías Retrofit

En el `build.gradle` del módulo `app`, añade las siguientes dependencias:

```gradle
implementation("com.squareup.retrofit2:retrofit:2.11.0")
implementation("com.squareup.retrofit2:converter-gson:2.11.0")
```

---

## Definir interfaz APIservice

Dentro de nuestro paquete `api` creamos la interfaz `APIservice` que define los endpoints de la API web:

```kotlin
interface APIservice {
    @GET
    suspend fun getMuncipios(@Url url: String): Response<MunicipiosResponse>
}
```

---

## Clases de datos para la respuesta JSON

La API devuelve un JSON con una lista de municipios. Crearemos estas clases para mapearla:

```kotlin
data class MunicipiosResponse(var municipios: List<Municipio>)

class Municipio(var CODIGOINE: String, var NOMBRE: String)
```

---

## Instancia Retrofit en BuscadorFragment

Creamos variables para el `RecyclerView` y el adaptador:

```kotlin
private lateinit var recy: RecyclerView
val adaptadorRecyclerView = adaptadorListaMunicipios()
```

En `onViewCreated` enlazamos los widgets:

```kotlin
recy = view.findViewById(R.id.rvMunicipios)
recy.layoutManager = LinearLayoutManager(context, LinearLayoutManager.VERTICAL, false)
recy.adapter = adaptadorRecyclerView
```

Método para crear la instancia de Retrofit:

```kotlin
private fun getRetrofit(): Retrofit {
    return Retrofit.Builder()
        .baseUrl("https://www.el-tiempo.net/api/json/v2/provincias/")
        .addConverterFactory(GsonConverterFactory.create())
        .build()
}
```

---

## Sobrescribir onResume para lanzar la búsqueda

```kotlin
override fun onResume() {
    super.onResume()
    busquedaMunicipios()
}
```

---

## Método para llamar a la API y actualizar el RecyclerView

```kotlin
private fun busquedaMunicipios() {
    prefs = requireActivity().getSharedPreferences("Preferencias", MODE_PRIVATE)
    val pref_spinner = prefs.getString("spinner", "30")
    CoroutineScope(Dispatchers.IO).launch {
        val call = getRetrofit().create(APIservice::class.java).getMuncipios(
            "$pref_spinner/municipios"
        )
        val tiempoAPI = call.body()
        if (call.isSuccessful) {
            val municipios = tiempoAPI?.municipios ?: emptyList()
            withContext(Dispatchers.Main) {
                adaptadorRecyclerView.changelist(municipios)
                adaptadorRecyclerView.notifyDataSetChanged()
            }
        }
    }
}
```

---

Con esto, cada vez que entres en el `BuscadorFragment`, se realizará la llamada a la API para cargar la lista de municipios basada en la provincia guardada en SharedPreferences (por defecto `"30"` para Murcia).

---
# Implementación del SearchView en BuscadorFragment

## 1. Implementar el listener

En la definición de la clase `BuscadorFragment`, añadimos la implementación del listener `SearchView.OnQueryTextListener`:

```kotlin
class BuscadorFragment : Fragment(), SearchView.OnQueryTextListener
````

Esto generará un error que se resuelve implementando los siguientes métodos:

```kotlin
override fun onQueryTextSubmit(query: String?): Boolean {
    if (!query.isNullOrEmpty()) {
        filtrarMunicipios(query)
    }

    // Ocultar teclado
    val imm = activity?.getSystemService(INPUT_METHOD_SERVICE) as InputMethodManager
    imm.hideSoftInputFromWindow(this.buscador.windowToken, 0)
    return true
}

override fun onQueryTextChange(newText: String?): Boolean {
    if (!newText.isNullOrEmpty()) {
        filtrarMunicipios(newText)
    }
    return true
}
```

---

## 2. Variable SearchView

Creamos una variable de tipo `SearchView` y la bindeamos en `onViewCreated`:

```kotlin
private lateinit var buscador: SearchView
```

En `onViewCreated`:

```kotlin
buscador = view.findViewById(R.id.svMunicipios)
buscador.setOnQueryTextListener(this)
```

---

## 3. Método para filtrar municipios

Este método envía el texto al adaptador para aplicar el filtro:

```kotlin
private fun filtrarMunicipios(query: String) {
    adaptadorRecyclerView.filter.filter(query)
}
```

---

Con esta implementación, el `SearchView` ya estará conectado con el `RecyclerView` y permitirá buscar municipios por nombre dinámicamente.

