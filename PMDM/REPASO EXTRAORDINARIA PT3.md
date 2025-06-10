
# 🧷 TabLayout + ViewPager2

## 🎯 ¿Por qué usar `TabLayout`?

Mostrar pestañas junto a una interfaz deslizante con `ViewPager2` es una excelente manera de indicar visualmente a los usuarios **en qué pantalla se encuentran**.

Android ofrece el widget `TabLayout` dentro del paquete **Material Components** (`com.google.android.material.tabs.TabLayout`) para este propósito.

---

## 🧱 Paso 1: Insertar `TabLayout` en el layout principal

Abre tu archivo `activity_main.xml` y añade el siguiente widget justo **encima** del `ViewPager2`:

```xml
<com.google.android.material.tabs.TabLayout
    android:id="@+id/tabLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintHorizontal_bias="0.0"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="parent"
    app:tabGravity="fill"
    app:tabMaxWidth="0dp"
    app:tabMode="fixed" />
````

---

## 🧱 Paso 2: Ajustar el `ViewPager2` para engancharlo al `TabLayout`

Modifica tu `ViewPager2` para que se ubique **debajo del TabLayout** y se adapte al espacio restante:

```xml
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintTop_toBottomOf="@id/tabLayout"
    app:layout_constraintBottom_toBottomOf="parent" />
```

---

# 📌 TabLayout y ViewPager2

## ¿Por qué usar TabLayout?

Mostrar pestañas junto con pantallas deslizables es una excelente forma de indicar a los usuarios en qué posición o sección de la aplicación se encuentran.

Android ofrece el widget `TabLayout` dentro del paquete **Material Components** para esta funcionalidad.

---

## 🔗 Conectar TabLayout con ViewPager2 usando `TabLayoutMediator`

Para enlazar el `TabLayout` con el `ViewPager2` y sincronizar ambos componentes, debes usar la clase `TabLayoutMediator`. Esto permite que las pestañas cambien cuando el usuario desliza entre pantallas o pulsa una pestaña.

---

## 🛠️ Implementación paso a paso

### 1. Crear variables en `MainActivity`

```kotlin
private lateinit var tabLayout: TabLayout
````

### 2. Enlazar el `TabLayout` en `onCreate()`

```kotlin
tabLayout = findViewById(R.id.tabLayout)
```

### 3. Utilizar `TabLayoutMediator` para conectar el TabLayout y ViewPager2

```kotlin
TabLayoutMediator(tabLayout, viewPager) { tab, position ->

    when (position) {
        0 -> {
            tab.text = "CONFIGURADOR"
        }
        1 -> {
            tab.text = "BUSCADOR MUNICIPIOS"
        }
    }

}.attach()
```

---

✅ Esto asegura que el texto y la posición de las pestañas estén siempre sincronizados con las páginas que muestra el `ViewPager2`.

---
Conectar una TabLayout con ViewPager2

Mostrar pestañas a la vez que usas las pantallas deslizable es una buena manera de hacerle saber a los usuarios en que posición están.

Android proporciona TabLayout. Un widget para mostrar las pestañas que forma parte del paquete Material.


https://aulavirtual.murciaeduca.es/pluginfile.php/7147544/mod_lesson/page_contents/52110/peek_2.gif

---

# 📋 Menú Superior (App Bar)
![image](https://github.com/user-attachments/assets/14f33556-46c7-4e16-ae3e-197db16af6ae)


La **app bar** (barra de aplicaciones) suele mostrar:

- El título de la app.
- Botones de acción.
- Un menú desplegable (menú de overflow) con opciones adicionales que no caben o no se quieren mostrar como botones.

---

## ¿Por qué usar la App Bar?

- Hace que tu app sea consistente con otras aplicaciones Android.
- Facilita que los usuarios aprendan a usar la app rápidamente.
- Está provista por la librería `appcompat`, que generalmente viene incluida en todos los proyectos Android.

---

## Dependencia en `build.gradle`

La librería que provee la app bar suele estar incluida por defecto, pero se puede verificar que esté en las dependencias:

```gradle
implementation(libs.androidx.appcompat)
````

---

## 📝 Layout del Menú

Para personalizar el menú, se crea un archivo XML dentro de:

```
/res/menu/
```

Este archivo debe tener:

* Un elemento raíz `<menu>`.
* Uno o varios elementos `<item>`, cada uno representando una opción del menú.

---

## Atributos principales de `<item>`

| Atributo               | Descripción                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| `android:id`           | ID único del elemento, para referenciarlo desde código.                                    |
| `android:title`        | Texto que se mostrará en el menú.                                                          |
| `android:icon`         | Icono asociado a la opción.                                                                |
| `android:showAsAction` | Define cómo se mostrará la opción en la barra: botón o menú overflow. Puede tomar valores: |
|                        | - `ifRoom`: mostrar como botón solo si hay espacio.                                        |
|                        | - `withText`: mostrar texto junto al icono si es botón.                                    |
|                        | - `never`: siempre en menú overflow.                                                       |
|                        | - `always`: siempre como botón (puede superponer otros si hay poco espacio).               |

---

## Ejemplo de menú XML

En este ejemplo, el menú contiene un solo ítem que es un icono de corazón que representará favoritos (por ejemplo, mostrar ítems guardados en SQLite):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/action_fav"
        android:title="@string/favoritos"
        android:icon="@drawable/listfav"
        android:orderInCategory="100"
        app:showAsAction="ifRoom|withText" />
</menu>
```

---
![image](https://github.com/user-attachments/assets/852e3a3f-6a7c-487e-bd3e-ffc499d8c0e0)


## Crear el archivo de menú

* Crea un nuevo resource file de tipo **menu** en el directorio `/res/menu`.
* Pega el código anterior.
* Luego, este menú podrá ser inflado desde la actividad para aparecer en la app bar.

---
## Importar Imagen
para nuestro menú necesitamos importar la imagen del corazón de
https://fonts.google.com/icons?selected=Material+Symbols+Outlined:favorite:FILL@0;wght@400;GRAD@0;opsz@24&icon.query=heart&icon.size=24&icon.color=%231f1f1f&icon.platform=android 

y lo hemos llamado "listfav"

---

## Incluir Toolbar en layout

 ya podemos añadir nuestro Toolbar en el layout de la actividad (activity_main). Para ello usamos el componente Toolbar:

``` xml
<androidx.appcompat.widget.Toolbar
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"/>
```

****modifica los contraints del tablayout para que ahora enganchen en la parte inferior de la toolbar

app:layout_constraintTop_toBottomOf="@id/toolbar"

---


# Asociar menú a la actividad principal

Para mostrar y gestionar un menú en la app bar de tu actividad principal, debes hacer lo siguiente:

---

## 1. Sobrescribir `onCreateOptionsMenu`

Este método infla el menú XML para mostrarlo en la barra de la aplicación.

```kotlin
override fun onCreateOptionsMenu(menu: Menu): Boolean {
    menuInflater.inflate(R.menu.menu_principal, menu)
    return true
}
````

* `R.menu.menu_principal` es el archivo XML del menú que creaste en `/res/menu/`.

---

## 2. Configurar la Toolbar en `onCreate`

Si usas una `Toolbar` personalizada en tu layout, debes indicarle a la actividad que la utilice como app bar con:

```kotlin
setSupportActionBar(findViewById(R.id.toolbar))
```

---

## 3. Gestionar eventos de selección con `onOptionsItemSelected`

Para responder cuando el usuario selecciona una opción del menú, sobreescribe este método:

```kotlin
override fun onOptionsItemSelected(item: MenuItem) = when (item.itemId) {
    R.id.action_fav -> {
        // Acción al pulsar el icono de favoritos
        true
    }
    else -> {
        super.onOptionsItemSelected(item)
    }
}
```

* Aquí puedes manejar las acciones que ocurran cuando el usuario pulse cada opción del menú.

---

Con esto, tu menú aparecerá en la app bar y responderá a las acciones del usuario.

---




