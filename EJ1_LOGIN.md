
# 🐍 **Python: Ejercicio 1 - Login**  

```python
from PySide6.QtWidgets import QApplication, QLabel, QLineEdit, QPushButton, QVBoxLayout, QWidget
```

### 📦 **Importación de módulos**  
Se importan los módulos necesarios de PySide6 para crear la interfaz gráfica:  

- **`QApplication`**: Inicializa y gestiona la aplicación.  
- **`QLabel`**: Muestra texto estático en la ventana.  
- **`QLineEdit`**: Permite al usuario ingresar texto.  
- **`QPushButton`**: Crea botones interactivos.  
- **`QVBoxLayout`**: Organiza widgets en un diseño vertical.  
- **`QWidget`**: Clase base para ventanas y widgets.  

---

```python
class LoginWindow(QWidget):
```

### 🖼️ **Definición de la ventana principal**  
- **`LoginWindow`**: Es una clase que hereda de `QWidget` y representa la ventana principal.  

---

```python
    def __init__(self):
```

### 🔧 **Constructor de la clase**  
Se configuran los elementos de la interfaz:  

```python
        super().__init__()
```
- **`super().__init__()`**: Llama al constructor de la clase base `QWidget`.  

```python
        self.setWindowTitle('Login')
```
- **`self.setWindowTitle('Login')`**: Configura el título de la ventana como *Login*.  

---

### 🛠️ **Creación de widgets**  

#### 1️⃣ **Etiqueta y campo de entrada para usuario:**  
```python
        self.user_label = QLabel('Usuario:', self)
        self.user_input = QLineEdit(self)
```
- **`QLabel`**: Muestra "Usuario:".  
- **`QLineEdit`**: Permite al usuario ingresar texto.  

#### 2️⃣ **Etiqueta y campo de entrada para contraseña:**  
```python
        self.password_label = QLabel('Contraseña:', self)
        self.password_input = QLineEdit(self)
        self.password_input.setEchoMode(QLineEdit.Password)
```
- **`QLabel`**: Muestra "Contraseña:".  
- **`QLineEdit`**: Permite al usuario ingresar texto.  
- **`setEchoMode(QLineEdit.Password)`**: Oculta el texto mientras se escribe.  

#### 3️⃣ **Botón de Login:**  
```python
        self.login_button = QPushButton('Login', self)
        self.login_button.clicked.connect(self.check_credentials)
```
- **`QPushButton`**: Crea un botón con la etiqueta "Login".  
- **`clicked.connect(self.check_credentials)`**: Conecta el clic a la función `check_credentials`.  

#### 4️⃣ **Etiqueta para mensajes dinámicos:**  
```python
        self.message_label = QLabel('', self)
```
- **`QLabel`**: Inicialmente vacía, se usa para mostrar mensajes de éxito o error.  

---

### 🧩 **Diseño de la ventana**  
```python
        layout = QVBoxLayout()
```
- **`QVBoxLayout`**: Organiza los widgets en un diseño vertical.  

#### Añade los widgets al diseño:  
```python
        layout.addWidget(self.user_label)
        layout.addWidget(self.user_input)
        layout.addWidget(self.password_label)
        layout.addWidget(self.password_input)
        layout.addWidget(self.login_button)
        layout.addWidget(self.message_label)
```
- Cada widget (etiquetas, campos y botón) se añade al layout.  

```python
        self.setLayout(layout)
```
- **`setLayout(layout)`**: Asigna el layout a la ventana principal.  

---

### 🔒 **Verificación de credenciales**  
```python
    def check_credentials(self):
        username = self.user_input.text()
        password = self.password_input.text()
```
- **`text()`**: Obtiene el texto ingresado en los campos de usuario y contraseña.  

#### Validación:  
```python
        if username == 'admin' and password == 'admin':
            self.message_label.setText('Login Correcto!')
            self.message_label.setStyleSheet('color: green')
```
- Si las credenciales son correctas:  
  - Se muestra el mensaje *"Login Correcto!"* en verde.  

```python
        else:
            self.message_label.setText('Login incorrecto. Inténtalo otra vez.')
            self.message_label.setStyleSheet('color: red')
```
- Si las credenciales son incorrectas:  
  - Se muestra el mensaje *"Login incorrecto."* en rojo.  

---

### 🏁 **Inicialización del programa**  
```python
if __name__ == '__main__':
    app = QApplication([])
    login_window = LoginWindow()
    login_window.show()
    app.exec()
```

1. **`QApplication([])`**: Inicializa la aplicación.  
2. **`LoginWindow()`**: Crea una instancia de la ventana de login.  
3. **`show()`**: Muestra la ventana en pantalla.  
4. **`exec()`**: Inicia el bucle de eventos.  


