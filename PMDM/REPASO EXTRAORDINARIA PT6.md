
# DetalleMunicipioActivity

## Objetivo
Crear una nueva actividad para mostrar los detalles del tiempo de un municipio específico, utilizando la API de [el-tiempo.net](https://www.el-tiempo.net).

---

## 1. Crear nueva actividad

Creamos una nueva **Empty Activity** llamada `DetalleMunicipioActivity`.

---

## 2. Layout (activity_detalle_municipio.xml)

Utilizamos un `ConstraintLayout` para mostrar:

- Descripción del cielo (`TextView`)
- Temperatura máxima y mínima (`TextView`)
- Imagen representativa del clima (`ImageView`)

IDs sugeridos:
- `tvCielo`
- `tvTempMax`
- `tvTempMin`
- `ivTiempo`

---

## Modificar layout de la actividad
Sobre este layout no hay mucho que explicar. Simplemente había que organizar todos lo items que posteriormente usaremos a mostrar la información.


![image](https://github.com/user-attachments/assets/0170cd43-5c22-4b01-86f5-03e5bf97dd69)



He usado estos id que posteriormente usaré para modificar su valor:

· tvCodigoMunicipio
· tvNombreMunicipio
· tvEstadoDelCielo
· tvTempMax
· tvTempMin

## 3. Código base de la actividad

```kotlin
class DetalleMunicipioActivity : AppCompatActivity() {

    private lateinit var cielo: TextView
    private lateinit var tempMax: TextView
    private lateinit var tempMin: TextView
    private lateinit var imagen: ImageView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_detalle_municipio)

        cielo = findViewById(R.id.tvCielo)
        tempMax = findViewById(R.id.tvTempMax)
        tempMin = findViewById(R.id.tvTempMin)
        imagen = findViewById(R.id.ivTiempo)

        val codProv = intent.getStringExtra("codProv")
        val codMunicipio = intent.getStringExtra("codMunicipio")

        recopilarInfoMunicipio(codProv, codMunicipio)
    }

    private fun recopilarInfoMunicipio(codProv: String?, codMunicipio: String?) {
        CoroutineScope(Dispatchers.IO).launch {
            val retrofit = Retrofit.Builder()
                .baseUrl("https://www.el-tiempo.net/api/json/v2/provincias/")
                .addConverterFactory(GsonConverterFactory.create())
                .build()

            val service = retrofit.create(APIservice::class.java)
            val call = service.getMuncipios("$codProv/municipios/$codMunicipio")
            val resultado = call.body()

            if (call.isSuccessful && resultado != null) {
                val cieloTexto = resultado.stateSky.descripcion
                val max = resultado.temperaturas.max
                val min = resultado.temperaturas.min

                withContext(Dispatchers.Main) {
                    cielo.text = cieloTexto
                    tempMax.text = "Máx: $max ºC"
                    tempMin.text = "Mín: $min ºC"
                    cargarImagenes()
                }
            }
        }
    }

    private fun cargarImagenes() {
        when {
            cielo.text.contains("Tormenta", ignoreCase = true) -> {
                imagen.setImageResource(R.drawable.tormenta)
            }
            cielo.text.contains("Nuboso", ignoreCase = true) ||
            cielo.text.contains("Cubierto", ignoreCase = true) -> {
                imagen.setImageResource(R.drawable.nubes)
            }
            cielo.text.contains("lluvia", ignoreCase = true) -> {
                imagen.setImageResource(R.drawable.lluvia)
            }
            else -> {
                imagen.setImageResource(R.drawable.sol)
            }
        }
    }
}
````

---

## 4. Datos esperados de la API

* **`stateSky.descripcion`**: descripción del cielo (ej: "Despejado", "Lluvia", "Tormenta", etc.)
* **`temperaturas.max`**: temperatura máxima
* **`temperaturas.min`**: temperatura mínima

---

## 5. Modelo de datos sugerido

```kotlin
data class MunicipioDetalleResponse(
    val stateSky: EstadoCielo,
    val temperaturas: Temperaturas
)

data class EstadoCielo(val descripcion: String)

data class Temperaturas(val max: String, val min: String)
```

---

## 6. Enlace desde otra actividad

Para lanzar `DetalleMunicipioActivity`:

```kotlin
val intent = Intent(context, DetalleMunicipioActivity::class.java)
intent.putExtra("codProv", "30")
intent.putExtra("codMunicipio", "30030")
startActivity(intent)
```

---

## 7. Recursos gráficos

Agrega imágenes en `res/drawable` con nombres como:

* `sol.png`
* `lluvia.png`
* `nubes.png`
* `tormenta.png`

---

Con esta configuración, al pulsar un municipio se mostrará una vista detallada del tiempo en esa localidad, incluyendo descripción del cielo, temperaturas y una imagen representativa.

# DetalleMunicipioActivity

## 📌 Objetivo

Mostrar los detalles meteorológicos de un municipio concreto, obtenidos a través de una llamada a la API de **el-tiempo.net**.

---

## 🛠️ Paso 1: Crear la nueva actividad

Crea una nueva **Empty Activity** llamada `DetalleMunicipioActivity`.

---

## 🧩 Paso 2: Layout con ConstraintLayout

Usa `ConstraintLayout` con los siguientes elementos (ejemplo visual):

- `TextView`: Código del municipio (`id: tvCodigo`)
- `TextView`: Nombre del municipio (`id: tvNombre`)
- `TextView`: Descripción del cielo (`id: tvEstadoDelCielo`)
- `TextView`: Temperatura máxima (`id: tvTempMax`)
- `TextView`: Temperatura mínima (`id: tvTempMin`)
- `ImageView`: Imagen del estado del cielo (`id: ivCielo`)

---

## 🧬 Paso 3: Variables en la actividad

```kotlin
private lateinit var codigoTextView: TextView
private lateinit var nombreTextView: TextView
private lateinit var cielo: TextView
private lateinit var tMax: TextView
private lateinit var tMin: TextView
private lateinit var imagen: ImageView
````

Aquí tienes un ejemplo completo del layout `activity_detalle_municipio.xml` usando **ConstraintLayout**, con todos los elementos necesarios para mostrar el detalle del municipio y el estado del tiempo:

---

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/layoutDetalle"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".DetalleMunicipioActivity">

    <!-- Código del Municipio -->
    <TextView
        android:id="@+id/tvCodigo"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Código del Municipio"
        android:textSize="16sp"
        android:textStyle="bold"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Nombre del Municipio -->
    <TextView
        android:id="@+id/tvNombre"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Nombre del Municipio"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintTop_toBottomOf="@id/tvCodigo"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="8dp" />

    <!-- Estado del Cielo -->
    <TextView
        android:id="@+id/tvEstadoDelCielo"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Estado del Cielo"
        android:textSize="16sp"
        app:layout_constraintTop_toBottomOf="@id/tvNombre"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

    <!-- Temperatura Máxima -->
    <TextView
        android:id="@+id/tvTempMax"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Temperatura Máxima"
        android:textSize="16sp"
        app:layout_constraintTop_toBottomOf="@id/tvEstadoDelCielo"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="8dp" />

    <!-- Temperatura Mínima -->
    <TextView
        android:id="@+id/tvTempMin"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Temperatura Mínima"
        android:textSize="16sp"
        app:layout_constraintTop_toBottomOf="@id/tvTempMax"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="4dp" />

    <!-- Imagen del Estado del Cielo -->
    <ImageView
        android:id="@+id/ivCielo"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:contentDescription="Imagen del cielo"
        android:scaleType="centerInside"
        app:layout_constraintTop_toBottomOf="@id/tvTempMin"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

---

### 🎯 Recomendaciones

* Asegúrate de tener los siguientes recursos en `res/drawable`:
  `sol.png`, `nubes.png`, `lluvia.png`, `tormenta.png`.


## 🔗 Paso 4: Inicializar y obtener datos del intent

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_detalle_municipio)

    // Binding
    codigoTextView = findViewById(R.id.tvCodigo)
    nombreTextView = findViewById(R.id.tvNombre)
    cielo = findViewById(R.id.tvEstadoDelCielo)
    tMax = findViewById(R.id.tvTempMax)
    tMin = findViewById(R.id.tvTempMin)
    imagen = findViewById(R.id.ivCielo)

    // Obtener datos del intent
    val municipioCodigo = intent.getStringExtra("municipio_codigo")
    val municipioNombre = intent.getStringExtra("municipio_nombre")
    val codProvincia = intent.getStringExtra("provincia_codigo")

    // Mostrar nombre y código
    codigoTextView.text = municipioCodigo
    nombreTextView.text = municipioNombre

    // Llamar a API
    recopilarInfoMunicipio(codProvincia, municipioCodigo)
}
```

---

## 🌐 Paso 5: Método `recopilarInfoMunicipio`

```kotlin
private fun recopilarInfoMunicipio(codProvincia: String?, municipioCodigo: String?) {
    CoroutineScope(Dispatchers.IO).launch {
        val retrofit = Retrofit.Builder()
            .baseUrl("https://www.el-tiempo.net/api/json/v2/provincias/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()

        val service = retrofit.create(APIservice::class.java)
        val call = service.getMuncipios("$codProvincia/municipios/$municipioCodigo")
        val response = call.body()

        if (call.isSuccessful && response != null) {
            val estadoCielo = response.stateSky.descripcion
            val max = response.temperaturas.max
            val min = response.temperaturas.min

            withContext(Dispatchers.Main) {
                cielo.text = estadoCielo
                tMax.text = "Máxima: $max ºC"
                tMin.text = "Mínima: $min ºC"
                cargarImagenes()
            }
        }
    }
}
```

---

## 🌤️ Paso 6: Método `cargarImagenes`

```kotlin
private fun cargarImagenes() {
    when {
        cielo.text.contains("Tormenta", ignoreCase = true) -> {
            imagen.setImageResource(R.drawable.tormenta)
        }
        cielo.text.contains("Nuboso", ignoreCase = true) ||
        cielo.text.contains("Cubierto", ignoreCase = true) -> {
            imagen.setImageResource(R.drawable.nubes)
        }
        cielo.text.contains("lluvia", ignoreCase = true) -> {
            imagen.setImageResource(R.drawable.lluvia)
        }
        else -> {
            imagen.setImageResource(R.drawable.sol)
        }
    }
}
```

---

## 📡 Paso 7: Modelo de datos

```kotlin
data class MunicipioDetalleResponse(
    val stateSky: EstadoCielo,
    val temperaturas: Temperaturas
)

data class EstadoCielo(val descripcion: String)

data class Temperaturas(val max: String, val min: String)
```

---

## 🚀 Paso 8: Cómo lanzar la actividad desde otro fragmento o activity

```kotlin
val intent = Intent(context, DetalleMunicipioActivity::class.java)
intent.putExtra("municipio_codigo", municipio.CODIGOINE)
intent.putExtra("municipio_nombre", municipio.NOMBRE)
intent.putExtra("provincia_codigo", "30") // Murcia por defecto
startActivity(intent)
```

---

## 🖼️ Paso 9: Recursos gráficos en `/res/drawable`

Agrega imágenes con estos nombres:

* `sol.png`
* `lluvia.png`
* `nubes.png`
* `tormenta.png`

---

Con todo esto implementado, `DetalleMunicipioActivity` podrá mostrar la información meteorológica detallada de un municipio específico usando datos reales de la API.


## 🔄 Enviar datos a otra actividad (DetalleMunicipioActivity)

### 🎯 Objetivo

Enviar los datos del municipio seleccionado desde el `RecyclerView` del `BuscadorFragment` a la nueva actividad `DetalleMunicipioActivity`.

---

### 1. 🔧 Modificar el Adaptador del RecyclerView

#### Añadir el Callback

En tu adaptador (`adaptadorListaMunicipios`), añade lo siguiente:

```kotlin
private lateinit var listener: AdaptadorCallback

interface AdaptadorCallback {
    fun lanzarActividad(municipio: Municipio)
}

fun setAdaptadorCallback(listener: AdaptadorCallback) {
    this.listener = listener
}
```

#### Añadir el Listener al `itemView`

Dentro del `onBindViewHolder`, añade:

```kotlin
holder.itemView.setOnClickListener {
    listener.lanzarActividad(item)
}
```

---

### 2. 🧠 Implementar el Callback en el `BuscadorFragment`

En `onViewCreated`, añade la implementación del `callback` y la lógica para lanzar la nueva actividad:

```kotlin
adaptadorRecyclerView.setAdaptadorCallback(object : adaptadorListaMunicipios.AdaptadorCallback {
    override fun lanzarActividad(municipio: Municipio) {
        val intent = Intent(context, DetalleMunicipioActivity::class.java)

        // Pasar los datos del municipio al Intent
        intent.putExtra("municipio_codigo", municipio.CODIGOINE)
        intent.putExtra("municipio_nombre", municipio.NOMBRE)

        // Abrir la actividad
        startActivity(intent)
    }
})
```

---

### ✅ Resultado

Cuando el usuario pulse sobre un municipio en el listado, se lanzará `DetalleMunicipioActivity` con los datos necesarios (`CODIGOINE` y `NOMBRE`) para realizar la petición a la API y mostrar la información del clima del municipio seleccionado.

---



## 🌤️ Más API - Obtener detalles del municipio

Una vez que ya lanzamos correctamente la actividad `DetalleMunicipioActivity`, vamos a **recoger la información del tiempo** usando Retrofit.

---
![image](https://github.com/user-attachments/assets/79cf17ff-b7bc-4c80-891c-accb24570d14)


### 🧱 1. Configurar Retrofit (mismo que en Buscador)

```kotlin
private fun getRetrofit(): Retrofit {
    return Retrofit.Builder()
        .baseUrl("https://www.el-tiempo.net/api/json/v2/provincias/")
        .addConverterFactory(GsonConverterFactory.create())
        .build()
}
```

---

### 📡 2. Añadir endpoint en APIservice

En el archivo `APIservice.kt`, añade el nuevo endpoint:

```kotlin
@GET
suspend fun getTiempoMuncipio(@Url url: String): Response<TiempoMunicipioResponse>
```

---

### 🧩 3. Crear modelos de respuesta

Para mapear la respuesta JSON de la API, crea estas clases:

```kotlin
class TiempoMunicipioResponse(
    var stateSky: StateSky,
    var temperaturas: Temperaturas
)

class StateSky(
    var description: String
)

class Temperaturas(
    var max: String,
    var min: String
)
```

---

### 🏁 4. Implementar `recopilarInfoMunicipio` en `DetalleMunicipioActivity`

```kotlin
private fun recopilarInfoMunicipio(municipioCodigo: String?) {
    CoroutineScope(Dispatchers.IO).launch {
        val call = getRetrofit().create(APIservice::class.java).getTiempoMuncipio(
            municipioCodigo?.substring(0, 2) + "/municipios/" + municipioCodigo?.substring(0, 5)
        )
        val tiempoAPI = call.body()
        if (call.isSuccessful) {
            withContext(Dispatchers.Main) {
                cielo.text = tiempoAPI?.stateSky?.description
                tMax.text = tiempoAPI?.temperaturas?.max
                tMin.text = tiempoAPI?.temperaturas?.min
                cargarImagenes()
            }
        }
    }
}
```

---

### ℹ️ Notas importantes

* **CODIGOINE:** Se utiliza para construir el endpoint.

  * Ejemplo: `30024`
  * Los dos primeros dígitos (`30`) son el código de provincia.
  * Los cinco primeros dígitos (`30024`) son el ID del municipio.

---

### ✅ Resultado

Con esto, cuando se pulse sobre un municipio, se abrirá la actividad detalle y se mostrará:

* Estado del cielo (`description`)
* Temperatura máxima
* Temperatura mínima
* Imagen del cielo acorde a la descripción

---


## 🖼️ Cargar Imágenes

Una vez recuperada la descripción del estado del cielo desde la API, podemos mostrar una **imagen correspondiente** en la interfaz de usuario.

### 🎯 Objetivo

Modificar la imagen del `ImageView` según el texto que aparece en `cielo.text`.

---

### 📦 Requisitos

* Añadir las imágenes al directorio `res/drawable/`:

  * `tormenta.png` o `.xml`
  * `cubierto.png`
  * `lluvia.png`
  * `sol.png`

---

### 🧠 Lógica del método

El método comprueba si el texto del estado del cielo contiene ciertas palabras clave, y en base a eso cambia el recurso de la imagen.

### 📄 Código completo

```kotlin
private fun cargarImagenes() {
    if (cielo.text.contains("Tormenta", ignoreCase = true)) {
        imagen.setImageResource(R.drawable.tormenta)
    } else if (
        cielo.text.contains("Nuboso", ignoreCase = true) ||
        cielo.text.contains("Cubierto", ignoreCase = true)
    ) {
        imagen.setImageResource(R.drawable.cubierto)
    } else if (cielo.text.contains("lluvia", ignoreCase = true)) {
        imagen.setImageResource(R.drawable.lluvia)
    } else {
        imagen.setImageResource(R.drawable.sol)
    }
}
```

---

### ✅ Resultado

El `ImageView` (`imagen`) de `DetalleMunicipioActivity` ahora mostrará:

* 🌩️ **Tormenta:** imagen `tormenta`
* ☁️ **Nuboso o Cubierto:** imagen `cubierto`
* 🌧️ **Lluvia:** imagen `lluvia`
* ☀️ **Cualquier otro caso:** imagen `sol`

---




