
# 🐍 Bloc de Notas en Python con Tkinter

```python
import tkinter as tk  # Importa la biblioteca tkinter para crear interfaces gráficas
from tkinter import filedialog, messagebox  # Importa los módulos filedialog y messagebox de tkinter
```

### Importación de Módulos
- `tkinter`: Biblioteca estándar de Python para crear interfaces gráficas.
- `filedialog`: Proporciona diálogos para abrir y guardar archivos.
- `messagebox`: Permite mostrar mensajes de alerta e información al usuario.

```python
class BlocDeNotas:
```

### Clase `BlocDeNotas`🗒 ✏
- Representa un editor de texto básico.
- Gestiona la ventana principal y sus funcionalidades.

```python
    def __init__(self, root):
        self.root = root  # Guarda la referencia a la ventana principal
        self.root.title("Editor de Texto")  # Establece el título de la ventana principal
        self.file_path = None  # Inicializa la ruta del archivo como None
```

#### Constructor
- Configura la ventana principal (`root`) y sus elementos.
- Inicializa `file_path` como `None`, indicando que no hay archivo abierto.

```python
        self.text_area = tk.Text(self.root, wrap='word')  # Crea un widget de área de texto con ajuste de palabras
        self.text_area.pack(expand=1, fill='both')  # Empaqueta el área de texto para que se expanda y llene la ventana
```

#### Área de Texto
- Crea un widget `Text` para editar texto con ajuste de palabras.
- Se expande y llena la ventana principal.

```python
        self.menu_bar = tk.Menu(self.root)  # Crea una barra de menú
        self.root.config(menu=self.menu_bar)  # Configura la barra de menú en la ventana principal
```

#### Barra de Menú 🗄
- Añade una barra de menú (`menu_bar`) a la ventana principal.

```python
        file_menu = tk.Menu(self.menu_bar, tearoff=0)  # Crea un menú desplegable sin opción de separar
        file_menu.add_command(label="Abrir Archivo", command=self.open_file)  # Añade el comando "Abrir" al menú
        file_menu.add_command(label="Guardar Archivo", command=self.save_file)  # Añade el comando "Guardar" al menú
        file_menu.add_command(label="Guardar Archivo como...", command=self.save_file_as)  # Añade el comando "Guardar como..." al menú
        file_menu.add_separator()  # Añade un separador al menú
        file_menu.add_command(label="Cerrar menú", command=self.close_file)  # Añade el comando "Cerrar" al menú
        self.menu_bar.add_cascade(label="Menú", menu=file_menu)  # Añade el menú desplegable a la barra de menú
```

#### Menú Archivo 📁
- Crea un menú desplegable con las opciones:
  - **Abrir Archivo**: Abre un archivo existente.
  - **Guardar Archivo**: Guarda el archivo actual.
  - **Guardar Archivo como...**: Guarda el archivo con un nuevo nombre.
  - **Cerrar menú**: Borra el contenido del área de texto y reinicia el estado.

---

### Métodos

```python
    def open_file(self):
```

#### `open_file`
- Abre un cuadro de diálogo para seleccionar un archivo.
- Lee y muestra su contenido en el área de texto.

```python
    def save_file(self):
```

#### `save_file`
- Guarda el contenido actual del área de texto en el archivo abierto.
- Si no hay un archivo abierto, llama a `save_file_as`.

```python
    def save_file_as(self):
```

#### `save_file_as`
- Abre un cuadro de diálogo para guardar el contenido del área de texto como un nuevo archivo.

```python
    def close_file(self):
```

#### `close_file`
- Borra el contenido del área de texto y reinicia el estado de la aplicación.

---

### Inicialización de la Aplicación

```python
if __name__ == "__main__":
    root = tk.Tk()  # Crea la ventana principal
    editor = BlocDeNotas(root)  # Crea una instancia de TextEditor
    root.mainloop()  # Inicia el bucle principal de la aplicación
```

- **Ventana Principal (`root`)**: Punto de entrada de la aplicación.
- **`BlocDeNotas(root)`**: Crea una instancia del editor de texto.
- **`mainloop`**: Inicia el bucle de eventos de la interfaz gráfica.
