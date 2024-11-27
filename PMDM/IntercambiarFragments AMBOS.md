# Intercambiar Fragments

Este programa es una aplicación de Android que permite intercambiar entre dos fragmentos (`FragmentUno` y `FragmentDos`) en la interfaz de usuario principal (`MainActivity`). A continuación, se detalla la funcionalidad de cada clase y su propósito:

### `MainActivity`
`MainActivity` es la actividad principal de la aplicación. Su función principal es gestionar la interfaz de usuario y manejar las interacciones del usuario. En este caso, `MainActivity`:

1. **Define dos botones** (`boton1` y `boton2`) que se utilizan para intercambiar entre los fragmentos.
2. **Crea instancias de los fragmentos** `FragmentUno` y `FragmentDos`.
3. **Inicializa el primer fragmento** (`FragmentUno`) y lo añade al contenedor de fragmentos (`R.id.fragment`) al iniciar la actividad.
4. **Configura los listeners de los botones** para reemplazar el fragmento actual con `FragmentUno` o `FragmentDos` cuando se hace clic en los botones correspondientes.

### `FragmentUno`
`FragmentUno` es una subclase de `Fragment` que representa una parte de la interfaz de usuario. Su función principal es inflar su propio diseño (`R.layout.fragment_uno`) cuando se crea la vista del fragmento.

### `FragmentDos`
`FragmentDos` es similar a `FragmentUno`, pero representa una parte diferente de la interfaz de usuario. Su función principal es inflar su propio diseño (`R.layout.fragment_dos`) cuando se crea la vista del fragmento.

### Resumen del flujo de trabajo
1. Al iniciar la aplicación, `MainActivity` añade `FragmentUno` al contenedor de fragmentos.
2. Cuando el usuario hace clic en `boton1`, `FragmentUno` se muestra en el contenedor de fragmentos.
3. Cuando el usuario hace clic en `boton2`, `FragmentDos` se muestra en el contenedor de fragmentos.

Este mecanismo permite al usuario intercambiar entre dos fragmentos diferentes en la misma actividad.


---

