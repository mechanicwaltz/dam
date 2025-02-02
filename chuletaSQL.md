¡Claro! Aquí tienes una **chuleta rápida de SQL** y las **acciones más comunes de CRUD** para que las tengas a mano mientras practicas.

---

### **Chuleta rápida de SQL**

#### 1. **Seleccionar Datos (`SELECT`)**
```sql
SELECT columna1, columna2 FROM tabla;   -- Selecciona columnas específicas
SELECT * FROM tabla;                   -- Selecciona todas las columnas
SELECT DISTINCT columna FROM tabla;    -- Selecciona valores únicos
SELECT columna1, columna2 FROM tabla WHERE condicion;  -- Filtra los resultados
```

#### 2. **Filtrar Datos (`WHERE`)**
```sql
SELECT * FROM tabla WHERE columna = 'valor';           -- Condición exacta
SELECT * FROM tabla WHERE columna LIKE '%valor%';      -- Búsqueda por patrón (contiene "valor")
SELECT * FROM tabla WHERE columna BETWEEN 10 AND 20;   -- Rango de valores
SELECT * FROM tabla WHERE columna IN (valor1, valor2); -- Lista de valores
SELECT * FROM tabla WHERE columna IS NULL;            -- Filtra valores nulos
```

#### 3. **Ordenar Resultados (`ORDER BY`)**
```sql
SELECT * FROM tabla ORDER BY columna ASC;  -- Orden ascendente
SELECT * FROM tabla ORDER BY columna DESC; -- Orden descendente
```

#### 4. **Insertar Datos (`INSERT INTO`)**
```sql
INSERT INTO tabla (columna1, columna2) VALUES (valor1, valor2);   -- Inserta valores en columnas específicas
INSERT INTO tabla VALUES (valor1, valor2, valor3);                 -- Inserta valores en todas las columnas (si se corresponden)
```

#### 5. **Actualizar Datos (`UPDATE`)**
```sql
UPDATE tabla SET columna1 = valor1, columna2 = valor2 WHERE condicion;  -- Actualiza registros
```

#### 6. **Eliminar Datos (`DELETE`)**
```sql
DELETE FROM tabla WHERE condicion;  -- Elimina registros con una condición
```

#### 7. **Contar Registros (`COUNT`)**
```sql
SELECT COUNT(*) FROM tabla;          -- Cuenta todos los registros
SELECT COUNT(columna) FROM tabla;    -- Cuenta los valores no nulos de una columna
SELECT COUNT(DISTINCT columna) FROM tabla;  -- Cuenta los valores únicos
```

#### 8. **Funciones Agregadas**
```sql
SELECT AVG(columna) FROM tabla;   -- Promedio
SELECT MIN(columna) FROM tabla;   -- Valor mínimo
SELECT MAX(columna) FROM tabla;   -- Valor máximo
SELECT SUM(columna) FROM tabla;   -- Suma
```

#### 9. **Joins (Unir Tablas)**
```sql
SELECT * FROM tabla1 INNER JOIN tabla2 ON tabla1.columna = tabla2.columna;  -- Une dos tablas por una columna común
SELECT * FROM tabla1 LEFT JOIN tabla2 ON tabla1.columna = tabla2.columna;   -- Une y conserva todos los registros de la primera tabla
SELECT * FROM tabla1 RIGHT JOIN tabla2 ON tabla1.columna = tabla2.columna;  -- Une y conserva todos los registros de la segunda tabla
```

---

### **Acciones CRUD en SQL**

#### **1. Crear (Create)**

**`INSERT INTO`**: Insertar un nuevo registro en una tabla.

```sql
INSERT INTO discos (titulo, artista, precio, disponible) VALUES ('Disco A', 'Artista A', 19.99, 1);
```

#### **2. Leer (Read)**

**`SELECT`**: Consultar datos de una o más tablas.

```sql
SELECT * FROM discos;  -- Seleccionar todos los discos
SELECT titulo, precio FROM discos WHERE artista = 'Artista A';  -- Filtrar por artista
SELECT * FROM discos WHERE precio > 20;  -- Filtrar discos con precio mayor a 20
```

#### **3. Actualizar (Update)**

**`UPDATE`**: Modificar datos existentes.

```sql
UPDATE discos SET precio = 25 WHERE id = 1;  -- Actualizar el precio del disco con ID 1
UPDATE discos SET disponible = 0 WHERE artista = 'Artista A';  -- Marcar como no disponible los discos de un artista
```

#### **4. Eliminar (Delete)**

**`DELETE`**: Eliminar registros de una tabla.

```sql
DELETE FROM discos WHERE id = 1;  -- Eliminar el disco con ID 1
DELETE FROM discos WHERE artista = 'Artista A';  -- Eliminar discos de un artista específico
```

#### **5. Comprobar Existencia (Existence Check)**

**`SELECT` + `COUNT`**: Verificar si existen registros que cumplen con una condición.

```sql
SELECT COUNT(*) FROM discos WHERE titulo = 'Disco A';  -- Verificar si existe el disco 'Disco A'
```

---

### **Consejos rápidos**

- **ALIAS**: Usar alias para simplificar las consultas.
  ```sql
  SELECT d.titulo, d.precio FROM discos d WHERE d.artista = 'Artista A';
  ```

- **DISTINCT**: Elimina duplicados en los resultados.
  ```sql
  SELECT DISTINCT artista FROM discos;
  ```

- **LIMIT**: Limitar el número de resultados.
  ```sql
  SELECT * FROM discos LIMIT 5;  -- Devuelve solo los primeros 5 discos
  ```

- **GROUP BY**: Agrupar resultados por un campo.
  ```sql
  SELECT artista, COUNT(*) FROM discos GROUP BY artista;  -- Cuenta los discos por cada artista
  ```

---

¡Espero que te sea útil esta chuleta! Si necesitas más ejemplos o tienes alguna pregunta, ¡me avisas!
