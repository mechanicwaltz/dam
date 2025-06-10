

# 🔄 ViewPager2 y Fragmentos en Android

## 📱 ¿Qué es ViewPager?

Muchas aplicaciones modernas utilizan el patrón de **pantallas deslizables** (swipe). Este patrón permite al usuario **navegar entre secciones** deslizando horizontalmente.

### 🔁 Evolución

- `ViewPager` fue el componente original.
- En **2019**, Google lanzó **ViewPager2** como reemplazo:
  - Basado en `RecyclerView`.
  - Incluido en **AndroidX**.
  - Mejora el rendimiento y la flexibilidad.
  - Soluciona varias limitaciones del ViewPager original.

---

## ⚙️ Configurar Fragmentos

Para que nuestros fragmentos funcionen con ViewPager2, solo necesitaremos el método `onCreateView`.

### 🧩 Ejemplo: Fragmento mínimo

```kotlin
class ConfiguradorFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_configurador, container, false)
    }
}
````

> 🔥 **Importante:** Elimina cualquier otro código generado automáticamente que no sea `onCreateView`.

---

## ✍️ Personaliza el Layout

Para saber **qué fragmento está en pantalla**, edita cada layout XML (`fragment_configurador.xml`, `fragment_buscador.xml`, etc.) y añade un texto identificativo.

### 📄 Ejemplo de layout (`fragment_configurador.xml`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <TextView
        android:id="@+id/texto_configurador"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Fragmento: Configurador"
        android:textSize="18sp"
        android:textStyle="bold" />
</FrameLayout>
```

> ✅ Repite el mismo proceso para el layout de cada fragmento, cambiando el texto según corresponda (por ejemplo, "Fragmento: Buscador").

---

Con esto, ya tienes configurados fragmentos básicos listos para usarse con **ViewPager2**.

---

# 🔄 ViewPager2 en Android

## 📲 ¿Qué es ViewPager?

Hoy en día, muchas aplicaciones Android utilizan un patrón de **pantalla deslizable** para mostrar secciones principales de forma intuitiva y fluida. Este patrón permite al usuario navegar entre diferentes secciones deslizando horizontalmente.

## 🔁 Evolución de ViewPager

- `ViewPager` fue la implementación original.
- Desde **2019**, Google recomienda usar **ViewPager2**, que:
  - Es parte de **AndroidX**.
  - Se basa en `RecyclerView`.
  - Corrige problemas de rendimiento y flexibilidad del `ViewPager` original.

---
Claro, aquí tienes el contenido en formato **Markdown**:

````markdown
# 🔄 ViewPager2 en Android

## 📲 ¿Qué es ViewPager?

Hoy en día, muchas aplicaciones Android utilizan un patrón de **pantalla deslizable** para mostrar secciones principales de forma intuitiva y fluida. Este patrón permite al usuario navegar entre diferentes secciones deslizando horizontalmente.

## 🔁 Evolución de ViewPager

- `ViewPager` fue la implementación original.
- Desde **2019**, Google recomienda usar **ViewPager2**, que:
  - Es parte de **AndroidX**.
  - Se basa en `RecyclerView`.
  - Corrige problemas de rendimiento y flexibilidad del `ViewPager` original.

---

## 🚀 Introducir ViewPager2

### 🛠️ Paso 1: Modificar el layout de `MainActivity`

Edita el archivo `activity_main.xml` para incluir un **ViewPager2**.

### 📄 Código XML:

```xml
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_constraintTop_toBottomOf="parent"
    app:layout_constraintBottom_toBottomOf="parent" />
````

> ✅ Este componente ocupará toda la pantalla y permitirá mostrar los fragmentos deslizables.

---

Aquí tienes el contenido formateado en **Markdown** para documentar correctamente la creación del **adaptador para ViewPager2** en Android:

````markdown
# 🧩 Adaptador del ViewPager2

## 📌 ¿Por qué necesitamos un adaptador?

El componente `ViewPager2` por sí solo es simplemente un `ViewGroup`. Para que pueda mostrar contenido (por ejemplo, fragmentos deslizables), necesita un **adaptador de datos** que le diga **qué mostrar y cuándo**.

---

## 🛠️ Crear el adaptador: `ViewPagerAdapter`

### 📁 Organización del proyecto

Para mantener el código limpio y organizado:

1. Dentro de tu paquete principal, crea un nuevo **paquete** llamado `adaptadores`.
2. Dentro del paquete `adaptadores`, crea una **nueva clase Kotlin** llamada `ViewPagerAdapter`.

---

## 🧱 Estructura del adaptador

El adaptador debe heredar de `FragmentStateAdapter`, que es el recomendado por Google para trabajar con `ViewPager2`.

### 📝 Ejemplo de implementación básica:

```kotlin
package com.tuapp.adaptadores

import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import androidx.viewpager2.adapter.FragmentStateAdapter
import com.tuapp.fragments.BuscadorFragment
import com.tuapp.fragments.ConfiguradorFragment

class ViewPagerAdapter(activity: FragmentActivity) : FragmentStateAdapter(activity) {

    override fun getItemCount(): Int = 2 // Número de fragmentos

    override fun createFragment(position: Int): Fragment {
        return when (position) {
            0 -> ConfiguradorFragment()
            1 -> BuscadorFragment()
            else -> throw IllegalStateException("Posición inválida")
        }
    }
}
````
![image](https://github.com/user-attachments/assets/1e906892-6dcc-4ef9-a232-082649ee2e06)
---

Aquí tienes ese contenido claramente explicado y estructurado en **Markdown**:

````markdown
# 🧩 Extender de `FragmentStateAdapter`

## 🛠️ Clase `ViewPagerAdapter`

La clase `ViewPagerAdapter` es un adaptador personalizado que permite gestionar los fragmentos que se mostrarán dentro del componente `ViewPager2`.

### 🧱 Estructura básica:

```kotlin
class ViewPagerAdapter(
    activity: AppCompatActivity,
    var itemsCount: Int
) : FragmentStateAdapter(activity) {

    override fun getItemCount(): Int {
        return itemsCount
    }

    override fun createFragment(position: Int): Fragment {
        // Aquí se devuelven los fragmentos según la posición
        return when (position) {
            0 -> ConfiguradorFragment()
            1 -> BuscadorFragment()
            else -> throw IllegalArgumentException("Posición inválida: $position")
        }
    }
}
````
![image](https://github.com/user-attachments/assets/06c84010-50f0-437e-a919-7bf64ebe7342)

---

## 📌 Detalles importantes

* **Parámetros del constructor**:

  * `activity`: se refiere a la actividad que contiene el `ViewPager2`.
  * `itemsCount`: es el número de fragmentos (pantallas) que se mostrarán.

* **Herencia**:

  * La clase hereda de `FragmentStateAdapter`, que es parte de **AndroidX** y está optimizada para trabajar con fragmentos y `RecyclerView`.

* **Métodos sobrescritos**:

  * `getItemCount()`: devuelve el número total de fragmentos que mostrará el ViewPager.
  * `createFragment(position: Int)`: devuelve el fragmento correspondiente según la posición (0, 1, etc.).

---

✅ Esta clase permite a `ViewPager2` saber **cuántas páginas hay** y **qué fragmento mostrar** en cada posición.

```
Aquí tienes esa información claramente estructurada en **Markdown**, perfecta para usar como guía o documentación:

````markdown
# ⚙️ Codificar el Adaptador `ViewPagerAdapter`

## 🧩 Métodos sobrescritos en `FragmentStateAdapter`

La clase `ViewPagerAdapter` necesita implementar dos métodos obligatorios:

---

### 🔢 1. `getItemCount()`

Este método simplemente devuelve el número de fragmentos que se deben mostrar. En nuestro caso, es el valor que hemos recibido como parámetro (`itemsCount`).

```kotlin
override fun getItemCount(): Int {
    return itemsCount
}
````

---

### 🔄 2. `createFragment(position: Int)`

Este método devuelve una instancia del fragmento que debe mostrarse **según la posición**. Utilizamos un `when` para controlar qué fragmento devolver:

```kotlin
override fun createFragment(position: Int): Fragment {
    return when (position) {
        0 -> ConfiguradorFragment()
        1 -> BuscadorFragment()
        else -> ConfiguradorFragment() // Fallback por defecto
    }
}
```

> ℹ️ En este ejemplo, si se solicita una posición inválida, se devolverá por defecto el `ConfiguradorFragment`.

---

### 🧱 Resultado final del adaptador:

```kotlin
class ViewPagerAdapter(
    activity: AppCompatActivity,
    private val itemsCount: Int
) : FragmentStateAdapter(activity) {

    override fun getItemCount(): Int {
        return itemsCount
    }

    override fun createFragment(position: Int): Fragment {
        return when (position) {
            0 -> ConfiguradorFragment()
            1 -> BuscadorFragment()
            else -> ConfiguradorFragment()
        }
    }
}
```

Aquí tienes la sección de **"Ensamblaje"** explicada paso a paso y en formato **Markdown**:

````markdown
# 🧩 Ensamblaje Final del ViewPager2

Una vez creado el `ViewPagerAdapter`, el último paso es **conectarlo con el `ViewPager2`** en tu `MainActivity`.

---

## 🧱 Paso 1: Definir una constante

Justo debajo de los `imports` en `MainActivity.kt`, define una constante que indique el número de pestañas (fragmentos):

```kotlin
const val NPESTAÑAS = 2
````

---

## 🧱 Paso 2: Declarar la variable ViewPager

Dentro de la clase `MainActivity`, declara una variable de tipo `ViewPager2`:

```kotlin
private lateinit var viewPager: ViewPager2
```

---

## 🧱 Paso 3: Inicializar el ViewPager y asignar el adaptador

En el método `onCreate` de tu `MainActivity`, enlaza el ViewPager del layout y asígnale el adaptador:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    viewPager = findViewById(R.id.viewpager)

    val adapta = ViewPagerAdapter(this, NPESTAÑAS)

    viewPager.adapter = adapta
}
```


✅ ¡Con esto, ya tienes tu `ViewPager2` funcionando con múltiples fragmentos deslizando entre ellos!
## Resultado:
![image](https://github.com/user-attachments/assets/1fba8def-ca9c-4646-b5e0-bca6579e6d63)


---




