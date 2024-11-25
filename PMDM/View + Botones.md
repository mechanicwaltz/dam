
---

## **1. ¿Qué es un `View`?**

En Android, un **`View`** es la clase base de todos los elementos de la interfaz gráfica. Todo lo que ves en la pantalla (como botones, textos, imágenes) es un `View` o una subclase de `View`.

Ejemplos de subclases de `View`:
- **Button**: Un botón que los usuarios pueden presionar.
- **TextView**: Para mostrar texto.
- **EditText**: Un campo de entrada de texto.
- **ImageView**: Para mostrar imágenes.

Cada `View` tiene propiedades como su tamaño, posición, color, etc., y puede responder a eventos del usuario (como clics o toques).

---

# Card View.

## **1. ¿Qué es un CardView?**

Un **`CardView`** es un contenedor (similar a un `LinearLayout` o `RelativeLayout`) que tiene un estilo de tarjeta con bordes redondeados, sombra y otros efectos visuales. Se usa comúnmente para mejorar la apariencia de elementos en una lista o en cualquier parte de la interfaz de usuario donde desees dar un estilo más moderno y atractivo.

---

## **2. Requisitos previos:**

Primero, debes asegurarte de tener la dependencia de **CardView** en tu proyecto. Si aún no lo tienes, añádelo en el archivo `build.gradle` de tu módulo (normalmente `app/build.gradle`):

```gradle
dependencies {
    implementation 'androidx.cardview:cardview:1.0.0'
}
```

---

## **3. Uso básico de CardView**

### **XML:**
En tu archivo de diseño XML, usa el **`CardView`** para envolver otros `Views` (como `TextView`, `ImageView`, etc.) y darle estilo de tarjeta.

```xml
<androidx.cardview.widget.CardView
    android:id="@+id/cardViewExample"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardCornerRadius="10dp"
    app:cardElevation="5dp"
    app:cardUseCompatPadding="true"
    android:layout_margin="10dp">

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp">

        <ImageView
            android:id="@+id/imageViewCard"
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:src="@drawable/sample_image"
            android:scaleType="centerCrop"/>

        <TextView
            android:id="@+id/textViewTitle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Título de la tarjeta"
            android:textSize="18sp"
            android:layout_marginTop="10dp"/>

        <TextView
            android:id="@+id/textViewDescription"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Descripción del contenido en la tarjeta."
            android:textSize="14sp"
            android:layout_marginTop="5dp"/>

    </LinearLayout>
</androidx.cardview.widget.CardView>
```

### Explicación de los atributos principales de **CardView**:
- **`app:cardCornerRadius="10dp"`**: Define el radio de los bordes redondeados de la tarjeta.
- **`app:cardElevation="5dp"`**: Da una sombra a la tarjeta, lo que crea una sensación de profundidad.
- **`app:cardUseCompatPadding="true"`**: Asegura que el padding se ajuste correctamente en dispositivos más antiguos.
- **`android:layout_margin="10dp"`**: Añade un margen entre la tarjeta y otros elementos en su contenedor.

---

## **4. Uso en Kotlin:**

Una vez que hayas definido tu `CardView` en el XML, puedes interactuar con él en tu actividad o fragmento a través del código Kotlin.

### **Ejemplo en Kotlin (MainActivity.kt):**

#### Kotlin:
```kotlin
package com.example.cardviewexample

import android.os.Bundle
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import androidx.recyclerview.widget.LinearLayoutManager
import com.example.cardviewexample.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Configurar ViewBinding
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        // CardView
        val cardView = findViewById<CardView>(R.id.cardViewExample)

        cardView.setOnClickListener {
            Toast.makeText(this, "CardView presionado", Toast.LENGTH_SHORT).show()
        }
    }
}
```

### Explicación:
- **`findViewById<CardView>(R.id.cardViewExample)`**: Accedemos al `CardView` mediante su ID.
- **`setOnClickListener`**: Al igual que con un botón o cualquier otro `View`, puedes agregar un listener para detectar cuando el `CardView` es presionado.

---

## **5. CardView en un RecyclerView**

El uso más común del **`CardView`** es dentro de un `RecyclerView` para mostrar una lista de elementos. En este caso, cada elemento de la lista estaría representado por un `CardView`.

### **Ejemplo de uso de CardView dentro de un RecyclerView**

#### a) **XML para RecyclerView con CardView**:
```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

#### b) **Item layout (item_card.xml)**:
Este será el diseño de cada elemento dentro de la lista.

```xml
<androidx.cardview.widget.CardView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardCornerRadius="8dp"
    app:cardElevation="4dp"
    app:cardUseCompatPadding="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/titleTextView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Título"
            android:textSize="18sp"/>

        <TextView
            android:id="@+id/descriptionTextView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Descripción del item"
            android:textSize="14sp"/>
    </LinearLayout>

</androidx.cardview.widget.CardView>
```

#### c) **Adaptador para RecyclerView (CardAdapter.kt)**:
```kotlin
package com.example.cardviewexample

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.cardview.widget.CardView
import androidx.recyclerview.widget.RecyclerView

class CardAdapter(private val items: List<String>) : RecyclerView.Adapter<CardAdapter.CardViewHolder>() {

    class CardViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val cardView: CardView = itemView.findViewById(R.id.cardViewExample)
        val titleTextView: TextView = itemView.findViewById(R.id.titleTextView)
        val descriptionTextView: TextView = itemView.findViewById(R.id.descriptionTextView)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CardViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_card, parent, false)
        return CardViewHolder(view)
    }

    override fun onBindViewHolder(holder: CardViewHolder, position: Int) {
        val item = items[position]
        holder.titleTextView.text = item
        holder.descriptionTextView.text = "Descripción del item $position"
        holder.cardView.setOnClickListener {
            // Acción cuando se toca el CardView
        }
    }

    override fun getItemCount(): Int = items.size
}
```

#### d) **Configurar RecyclerView en la actividad:**

```kotlin
val recyclerView = findViewById<RecyclerView>(R.id.recyclerView)
val items = listOf("Item 1", "Item 2", "Item 3", "Item 4")

val adapter = CardAdapter(items)
recyclerView.layoutManager = LinearLayoutManager(this)
recyclerView.adapter = adapter
```

---

## **6. Resumen de propiedades y ventajas del CardView:**

- **`cardCornerRadius`**: Redondea las esquinas de la tarjeta.
- **`cardElevation`**: Agrega sombra, creando un efecto de profundidad.
- **`cardUseCompatPadding`**: Ajusta el padding en versiones anteriores de Android.
- **`LinearLayout`, `RelativeLayout` o cualquier otro contenedor**: Puedes poner dentro del `CardView` cualquier tipo de diseño de vistas.

**Ventajas**:
- **Estética atractiva**: Las tarjetas le dan un diseño más moderno y visualmente atractivo a tu UI.
- **Organización**: Es fácil organizar el contenido dentro de las tarjetas y darle formato con bordes, sombras, y otros efectos.

---

## **2. Botones (`Button`)**

Un **`Button`** es un `View` que los usuarios pueden pulsar para realizar una acción.

### Uso básico:

#### XML:
```xml
<Button
    android:id="@+id/btnExample"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Presionar" />
```

#### Kotlin:
```kotlin
val button = findViewById<Button>(R.id.btnExample)

button.setOnClickListener {
    Toast.makeText(this, "Botón presionado", Toast.LENGTH_SHORT).show()
}
```

### Explicación:
- **`setOnClickListener`**: Es un **Listener** (escucha) que detecta cuando el botón es presionado.
- Dentro de `setOnClickListener`, defines lo que debe pasar cuando el botón es clickeado (en este caso, se muestra un mensaje en pantalla con `Toast`).

---

## **3. TextViews**

Un **`TextView`** es un `View` para mostrar texto estático o dinámico.

### Uso básico:

#### XML:
```xml
<TextView
    android:id="@+id/tvExample"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hola, mundo" />
```

#### Kotlin:
```kotlin
val textView = findViewById<TextView>(R.id.tvExample)

// Cambiar el texto dinámicamente
textView.text = "Texto actualizado"
```

### Explicación:
- **`text`**: Propiedad que establece o recupera el texto que se muestra en el TextView.

---

## **4. Eventos y Listeners**

Los **event listeners** son funciones que "escuchan" eventos que ocurren en un `View` y ejecutan acciones cuando se detecta un evento.

### Ejemplo de listeners comunes:

#### a) `setOnClickListener`:
Detecta cuando se presiona un `View` (como un botón):
```kotlin
button.setOnClickListener {
    println("Botón presionado")
}
```

#### b) `setOnLongClickListener`:
Detecta cuando el usuario mantiene presionado el `View`:
```kotlin
button.setOnLongClickListener {
    println("Botón mantenido")
    true // Indica que el evento fue consumido.
}
```

#### c) `setOnFocusChangeListener`:
Detecta cuando un `View` gana o pierde el foco (útil en campos de texto):
```kotlin
editText.setOnFocusChangeListener { view, hasFocus ->
    if (hasFocus) {
        println("El campo de texto tiene foco")
    } else {
        println("El campo de texto perdió el foco")
    }
}
```

#### d) `setOnTouchListener`:
Detecta toques en el `View`, incluyendo gestos más complejos.
```kotlin
button.setOnTouchListener { view, motionEvent ->
    println("Toque detectado: ${motionEvent.action}")
    true
}
```

---

## **5. EditText (Campos de texto)**

Un **`EditText`** permite a los usuarios ingresar texto.

### Uso básico:

#### XML:
```xml
<EditText
    android:id="@+id/etExample"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Introduce tu nombre" />
```

#### Kotlin:
```kotlin
val editText = findViewById<EditText>(R.id.etExample)

// Obtener el texto ingresado por el usuario
val textoIngresado = editText.text.toString()
println("Texto ingresado: $textoIngresado")
```

### Explicación:
- **`hint`**: Texto que aparece como guía antes de que el usuario escriba.
- **`text`**: Propiedad que contiene el texto ingresado.

---

## **6. ImageView**

Un **`ImageView`** muestra imágenes en la interfaz.

### Uso básico:

#### XML:
```xml
<ImageView
    android:id="@+id/ivExample"
    android:layout_width="100dp"
    android:layout_height="100dp"
    android:src="@drawable/ic_launcher_foreground" />
```

#### Kotlin:
```kotlin
val imageView = findViewById<ImageView>(R.id.ivExample)

// Cambiar la imagen programáticamente
imageView.setImageResource(R.drawable.otro_recurso)
```

---

## **7. Interacciones complejas entre vistas**

Puedes combinar `View` y `Listeners` para construir interacciones. Por ejemplo:

### **Ejemplo: Botón que actualiza un TextView con texto ingresado en un EditText**

#### XML:
```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Introduce tu nombre" />

    <Button
        android:id="@+id/btnGreet"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Saludar" />

    <TextView
        android:id="@+id/tvGreeting"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="¡Hola!" />
</LinearLayout>
```

#### Kotlin:
```kotlin
val editText = findViewById<EditText>(R.id.etName)
val button = findViewById<Button>(R.id.btnGreet)
val textView = findViewById<TextView>(R.id.tvGreeting)

button.setOnClickListener {
    val name = editText.text.toString()
    if (name.isNotBlank()) {
        textView.text = "¡Hola, $name!"
    } else {
        textView.text = "Por favor, introduce tu nombre"
    }
}
```

---

## **8. Ventajas de los Listeners**

1. **Interactividad**: Permiten responder a eventos como clics o cambios.
2. **Flexibilidad**: Puedes agregar diferentes comportamientos en cada `View`.
3. **Dinamismo**: Los usuarios pueden interactuar en tiempo real.




---

