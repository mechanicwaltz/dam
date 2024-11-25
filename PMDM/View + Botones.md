
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

