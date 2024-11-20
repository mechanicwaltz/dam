# 🐍 **Python: Ejemplo de QMainWindow con QDockWidget y QTextEdit**  

```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
```
### 🌟 **Definición de la ventana principal**  
Se define una clase `MainWindow` que hereda de `QMainWindow`, representando la ventana principal de la aplicación.  

---

### 🔧 **Configuración de la ventana principal**  
```python
        self.setWindowTitle("Aplicación con Dock y QTextEdit")
        self.setGeometry(100, 100, 800, 600)
```
- **`setWindowTitle()`**: Establece el título de la ventana.  
- **`setGeometry()`**: Configura las dimensiones iniciales de la ventana (posición y tamaño).  

---

### 🛠️ **Creación del Dock y QTextEdit**  
```python
        self.dock = QDockWidget("Dock", self)
        self.dock.setAllowedAreas(Qt.TopDockWidgetArea)
        self.text_edit = QTextEdit()
        self.dock.setWidget(self.text_edit)
        self.addDockWidget(Qt.TopDockWidgetArea, self.dock)
```
- **`QDockWidget`**: Añade un widget acoplable (dock) titulado "Dock".  
- **`setAllowedAreas(Qt.TopDockWidgetArea)`**: Restringe el área del dock a la parte superior.  
- **`QTextEdit`**: Campo de texto editable que se coloca dentro del dock.  
- **`addDockWidget()`**: Añade el dock a la ventana principal.  

---

### 🖱️ **Acciones de la interfaz**  

#### Acción "Imprimir en dock"  
```python
        self.print_action = QAction("Imprimir en dock", self)
        self.print_action.setShortcut(QKeySequence("Ctrl+P"))
        self.print_action.setStatusTip("Al ejecutar esta acción, se añadirá el texto...")
        self.print_action.triggered.connect(self.print_in_dock)
```
- **`QAction`**: Crea una acción interactiva con texto y funcionalidad.  
- **`setShortcut()`**: Asigna el atajo de teclado *Ctrl+P*.  
- **`setStatusTip()`**: Proporciona una descripción breve sobre la acción.  
- **`triggered.connect()`**: Conecta la acción al método `print_in_dock`.  

#### Acción "¿Qué es esto?"  
```python
        self.what_is_this_action = QAction("¿Qué es esto?", self)
        self.what_is_this_action.setStatusTip("Este es un botón que muestra información adicional.")
        self.what_is_this_action.triggered.connect(self.show_message)
```
- Similar a la anterior, conecta un botón que muestra información adicional con el método `show_message`.  

---

### 🛠️ **Barra de herramientas y menú**  
```python
        toolbar = QToolBar("Barra de herramientas")
        toolbar.addAction(self.print_action)
        toolbar.addAction(self.what_is_this_action)
        self.addToolBar(toolbar)
```
- **`QToolBar`**: Crea una barra de herramientas con botones.  
- **`addAction()`**: Añade acciones a la barra.  

```python
        menu = self.menuBar().addMenu("Menú")
        menu.addAction(self.print_action)
```
- **`menuBar().addMenu()`**: Crea un menú desplegable con acciones.  

---

### 📐 **Diseño del layout principal**  
```python
        central_widget = QWidget()
        layout = QVBoxLayout()
        layout.addWidget(self.dock)
        layout.addWidget(self.main_component)
        central_widget.setLayout(layout)
        self.setCentralWidget(central_widget)
```
- **`QVBoxLayout`**: Organización vertical de widgets.  
- **`setCentralWidget()`**: Establece el widget principal para el diseño.  

---

### ⚙️ **Métodos de interacción**  

#### Método `print_in_dock()`  
```python
    def print_in_dock(self):
        self.text_edit.append("Has pulsado una acción")
        QMessageBox.information(self, "Mensaje", "Se añadirá el texto...")
```
- Añade texto al `QTextEdit` y muestra un cuadro de diálogo con información.  

#### Método `show_message()`  
```python
    def show_message(self):
        QMessageBox.information(self, "Mensaje", "Este es un botón que muestra información adicional.")
```
- Muestra un mensaje emergente con información adicional.  

---

### 🏁 **Punto de entrada del programa**  
```python
if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```
1. **`QApplication`**: Inicializa la aplicación.  
2. **`MainWindow()`**: Crea la ventana principal.  
3. **`show()`**: Muestra la ventana.  
4. **`exec_()`**: Inicia el bucle de eventos.  

