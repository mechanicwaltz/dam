# Leer un archivo JSON: 
### Para leer un archivo JSON, primero obtenemos el contenido del archivo como una cadena:
```
public static String leerArchivo(String rutaArchivo) throws IOException {
    return new String(Files.readAllBytes(Paths.get(rutaArchivo)));
}
```
### Obtener el JSONObject:
#### Una vez que tenemos el contenido del archivo, lo convertimos en un objeto JSONObject:

```
public static JSONObject obtenerJSONObject(String rutaArchivo) throws IOException {
    String contenido = leerArchivo(rutaArchivo);
    return new JSONObject(contenido);
}
```

### Obtener el JSONArray:
#### Para obtener un JSONArray de un JSONObject, utilizamos el siguiente método:
```
public static JSONArray obtenerJSONArray(JSONObject jsonObject, String clave) {
    return jsonObject.getJSONArray(clave);
}
```

### Leer cada elemento del JSONArray: 
#### Para leer cada elemento del JSONArray, se utiliza un bucle for que los recorra:

```
JSONObject jsonObject = obtenerJSONObject(rutaArchivo);
JSONArray jsonArray = obtenerJSONArray(jsonObject, "nodosHijo");

for (int i = 0; i < jsonArray.length(); i++) {
    JSONObject elemento = jsonArray.getJSONObject(i);
    String valor = elemento.getString("nombre");
    
    // En el caso de tener un elemento con más elementos
    JSONObject elementoConHijos = elemento.getJSONObject("elementoConHijos");
    String subValor = elementoConHijos.getString("subElemento");
}
```


# Escribir JSON

### Crear un JSONObject:
#### Para crear un JSONObject, simplemente instanciamos un nuevo objeto:

```
JSONObject jsonObject = new JSONObject();
```


### Crear un JSONArray y añadirlo al JSONObject:
```
JSONArray jsonArray = new JSONArray();
jsonArray.put(jsonObject);

JSONObject jsonFinal = new JSONObject();
jsonFinal.put("personas", jsonArray);
```

### Escribir el JSONObject en un archivo
#### Para escribir el JSONObject en un archivo, utilizamos el siguiente método:

```
public static void escribirJSON(String rutaArchivo, JSONObject jsonObject) throws IOException {
    Files.write(Paths.get(rutaArchivo), jsonObject.toString(4).getBytes());
}
```

### Ejemplo de JSON
#### Los pasos para la escritura del JSON son genéricos, por lo que para obtener un resultado coherente reemplazamos las variables y nombres de elementos por los siguientes:

```
{
    "personas": [
        {
            "nombre": "Juan"
        }
    ]
}
```
# Teoría (meter esto en XML también)
### Elemento raíz
#### el "elemento raíz" en un archivo XML es el elemento más externo que encapsula todo el contenido del XML. Es el primer y único elemento que aparece al inicio y al final del archivo, y todos los demás elementos deben estar dentro de él.
ejemplo: 
```
{
    "libros": [
        {
            "titulo": "Cien años de soledad",
            "autor": "Gabriel García Márquez",
            "anio": 1967
        },
        {
            "titulo": "Don Quijote de la Mancha",
            "autor": "Miguel de Cervantes",
            "anio": 1605
        }
    ]
}

```
### Si especificas "biblioteca" como el nombre del elemento raíz, el XML resultante podría verse así:
```
<biblioteca>
    <libros>
        <libro>
            <titulo>Cien años de soledad</titulo>
            <autor>Gabriel García Márquez</autor>
            <anio>1967</anio>
        </libro>
        <libro>
            <titulo>Don Quijote de la Mancha</titulo>
            <autor>Miguel de Cervantes</autor>
            <anio>1605</anio>
        </libro>
    </libros>
</biblioteca>
```

Aquí:

· `biblioteca` es el elemento raíz que contiene todo el contenido del XML.

· `libros` es un elemento dentro del elemento raíz que agrupa una lista de `libro`.

·  Cada `libro` tiene elementos hijos como `titulo`, `autor`, y `anio`.

# Metodos Auxiliares

## Conversor genérico de JSON a XML: 

```
import org.json.JSONObject;
import org.json.XML;

import javax.xml.transform.*;
import javax.xml.transform.stream.StreamResult;
import javax.xml.transform.stream.StreamSource;
import java.io.IOException;
import java.io.StringReader;
import java.io.StringWriter;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class JSONToXMLConverter {

    public static void convertJsonToXml(String rutaFicheroJSON, String rutaFicheroXML, String rootElementName) {
        try {
            // Leer el archivo JSON
            String contenidoJson = new String(Files.readAllBytes(Paths.get(rutaFicheroJSON)));

            // Convertir el contenido en un objeto JSONObject
            JSONObject jsonObject = new JSONObject(contenidoJson);

            // Crear un nuevo JSONObject con el elemento raíz especificado
            JSONObject root = new JSONObject();
            root.put(rootElementName, jsonObject);

            // Transformar el JSON a XML
            String xml = XML.toString(root);

            // Imprimir el XML para depuración
            System.out.println("XML generado:");
            System.out.println(xml);

            // Formatear el XML
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            transformer.setOutputProperty("{http://xml.apache.org/xslt}indent-amount", "4");
            StreamSource source = new StreamSource(new StringReader(xml));
            StringWriter stringWriter = new StringWriter();
            StreamResult result = new StreamResult(stringWriter);
            transformer.transform(source, result);
            String formattedXml = stringWriter.toString();

            // Escribir el XML en un nuevo archivo
            Files.write(Paths.get(rutaFicheroXML), formattedXml.getBytes(), StandardOpenOption.CREATE);

            // Mostrar el XML en la terminal
            System.out.println("Contenido del XML formateado:");
            System.out.println(formattedXml);

        } catch (IOException | TransformerException e) {
            System.out.println("Error al leer el archivo JSON o escribir el archivo XML: " + e.getMessage());
        }
    }


```
### Ejemplo de uso: 
```
    public static void main(String[] args) {
        // Ejemplo de uso del método
        convertJsonToXml("src/main/resources/ejemplo.json", "src/main/resources/ejemplo.xml", "root");
    }
}
```
## Conversor genérico de XML a JSON: 

```
import org.json.JSONObject;
import org.json.XML;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class XmlToJsonConverter {

    public static void main(String[] args) {
        String rutaFicheroXML = "src/main/resources/example.xml";
        String rutaFicheroJSON = "src/main/resources/example.json";
        convertirXMLaJSON(rutaFicheroXML, rutaFicheroJSON);
    }

    public static void convertirXMLaJSON(String rutaFicheroXML, String rutaFicheroJSON) {
        try {
            // Leer el archivo XML
            String contenidoXML = new String(Files.readAllBytes(Paths.get(rutaFicheroXML)));

            // Convertir el contenido en un objeto JSONObject
            JSONObject jsonObject = XML.toJSONObject(contenidoXML);

            // Escribir el JSON en el fichero
            Files.write(Paths.get(rutaFicheroJSON), jsonObject.toString(4).getBytes());

            // Mostrar el JSON en la terminal
            System.out.println("Contenido del JSON generado:");
            System.out.println(jsonObject.toString(4));

        } catch (IOException e) {
            System.out.println("Error al leer el archivo XML o escribir el archivo JSON: " + e.getMessage());
        }
    }
}
```

## Pasar JSON a TXT:

```
import org.json.JSONArray;
import org.json.JSONObject;

import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class JsonToTxtConverter {

    public static void convertirJSONaTXT(String rutaFicheroJSON, String rutaFicheroTXT) {
        try {
            // Leer el archivo JSON
            String contenidoJSON = new String(Files.readAllBytes(Paths.get(rutaFicheroJSON)));
            JSONObject jsonObject = new JSONObject(contenidoJSON);

            // Crear un FileWriter para escribir en el archivo de texto
            try (FileWriter writer = new FileWriter(rutaFicheroTXT)) {
                // Recorrer las claves del JSONObject
                for (String key : jsonObject.keySet()) {
                    Object value = jsonObject.get(key);
                    writer.write(key + ": " + value.toString() + "\n");
                }
            }

            System.out.println("Archivo TXT generado correctamente.");

        } catch (IOException e) {
            System.out.println("Error al leer el archivo JSON o escribir el archivo TXT: " + e.getMessage());
        }
    }
}
```


## Pasar de TXT a JSON:
```
import org.json.JSONObject;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class TxtToJsonConverter {

    public static void convertirTXTaJSON(String rutaFicheroTXT, String rutaFicheroJSON) {
        try {
            // Leer el archivo de texto
            List<String> lineas = Files.readAllLines(Paths.get(rutaFicheroTXT));
            JSONObject jsonObject = new JSONObject();

            // Recorrer cada línea del archivo de texto
            for (String linea : lineas) {
                // Dividir la línea en clave y valor
                String[] partes = linea.split(": ");
                if (partes.length == 2) {
                    String key = partes[0].trim();
                    String value = partes[1].trim();
                    jsonObject.put(key, value);
                }
            }

            // Escribir el JSON en el archivo
            Files.write(Paths.get(rutaFicheroJSON), jsonObject.toString(4).getBytes());

            System.out.println("Archivo JSON generado correctamente.");

        } catch (IOException e) {
            System.out.println("Error al leer el archivo TXT o escribir el archivo JSON: " + e.getMessage());
        }
    }
}
```




