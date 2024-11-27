# PRACTICA: ActionBar

Este programa es una aplicación Android que utiliza `ViewPager2` y `TabLayout` para mostrar diferentes fragmentos en una interfaz de usuario con pestañas. Aquí hay una descripción general de cómo funciona:

1. **MainActivity**:
   - Configura la actividad principal con un `Toolbar`, un `ViewPager2` y un `TabLayout`.
   - El `Toolbar` principal se configura sin título y se infla con un menú.
   - Un segundo `Toolbar` (`tbCard`) se configura con un título y un menú específico, manejando eventos de clic en los elementos del menú.
   - El `ViewPager2` se configura con un adaptador (`ViewPagerAdaptador`) que gestiona 6 páginas y se orienta verticalmente.
   - El `TabLayout` se sincroniza con el `ViewPager2` usando `TabLayoutMediator`, estableciendo el texto de cada pestaña según su posición.

2. **Animales Fragment**:
   - Un fragmento que puede recibir dos parámetros opcionales (`param1` y `param2`).
   - Infla su layout desde `fragment_animales.xml`.
   - Proporciona un método de fábrica `newInstance` para crear nuevas instancias del fragmento con los parámetros especificados.

3. **Plantas Fragment**:
   - Similar al fragmento `Animales`, puede recibir dos parámetros opcionales (`param1` y `param2`).
   - Infla su layout desde `fragment_plantas.xml`.
   - Proporciona un método de fábrica `newInstance` para crear nuevas instancias del fragmento con los parámetros especificados.

4. **ViewPagerAdaptador**:
   - Un adaptador para `ViewPager2` que gestiona la creación de fragmentos según la posición.
   - Devuelve el número de elementos (páginas) y el fragmento correspondiente a cada posición.


## XML:


### `activity_main.xml`
Este archivo define el layout de la actividad principal (`MainActivity`). Incluye un `Toolbar`, un `ViewPager2` y un `TabLayout`.

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbarPrincipal"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <com.google.android.material.tabs.TabLayout
            android:id="@+id/appbartabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMode="fixed" />
    </com.google.android.material.appbar.AppBarLayout>

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

### `fragment_animales.xml`
Este archivo define el layout del fragmento `Animales`.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- Contenido del fragmento Animales -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Fragmento Animales"
        android:textSize="24sp" />

</LinearLayout>
```

### `fragment_plantas.xml`
Este archivo define el layout del fragmento `Plantas`.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- Contenido del fragmento Plantas -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Fragmento Plantas"
        android:textSize="24sp" />

</LinearLayout>
```

### `menu.xml`
Este archivo define el menú de opciones de la actividad principal.

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/action_file"
        android:title="Archivo"
        android:icon="@drawable/ic_file"
        app:showAsAction="ifRoom" />
    <item
        android:id="@+id/action_buscar"
        android:title="Buscar"
        android:icon="@drawable/ic_search"
        app:showAsAction="ifRoom" />
    <item
        android:id="@+id/action_opciones"
        android:title="Opciones"
        android:icon="@drawable/ic_options"
        app:showAsAction="ifRoom" />
    <item
        android:id="@+id/action_salir"
        android:title="Salir"
        android:icon="@drawable/ic_exit"
        app:showAsAction="ifRoom" />
</menu>
```

### `menu_tarjeta.xml`
Este archivo define el menú del `Toolbar` secundario (`tbCard`).

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/action_borrar"
        android:title="Borrar"
        android:icon="@drawable/ic_delete"
        app:showAsAction="ifRoom" />
</menu>
```

Estos archivos XML definen la estructura y el contenido visual de la aplicación, incluyendo los layouts de las actividades y fragmentos, así como los menús de opciones.


En resumen, la aplicación muestra diferentes fragmentos (`Animales` y `Plantas`) en un `ViewPager2` con pestañas (`TabLayout`), permitiendo al usuario navegar entre ellos. Los `Toolbar` proporcionan menús con acciones específicas.
