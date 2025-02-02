```
import java.sql.*;
import java.util.ArrayList;

public class DiscoDAO {
    private Connection conexion;

    public DiscoDAO(Connection conexion) {
        this.conexion = conexion;
    }

    public boolean insertarDiscoGuillermo(Disco disco) {
        String sql = "INSERT INTO discos (titulo, artista, precio, disponible, fecha_lanzamiento) VALUES (?, ?, ?, ?, ?)";
        try (PreparedStatement stmt = conexion.prepareStatement(sql)) {
            stmt.setString(1, disco.getTitulo());
            stmt.setString(2, disco.getArtista());
            stmt.setDouble(3, disco.getPrecio());
            stmt.setBoolean(4, disco.isDisponible());
            stmt.setDate(5, Date.valueOf(disco.getFechaLanzamiento()));
            return stmt.executeUpdate() > 0;
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }

    public boolean actualizarDiscoGuillermo(Disco disco) {
        String sql = "UPDATE discos SET titulo = ?, artista = ?, precio = ?, disponible = ?, fecha_lanzamiento = ? WHERE id = ?";
        try (PreparedStatement stmt = conexion.prepareStatement(sql)) {
            stmt.setString(1, disco.getTitulo());
            stmt.setString(2, disco.getArtista());
            stmt.setDouble(3, disco.getPrecio());
            stmt.setBoolean(4, disco.isDisponible());
            stmt.setDate(5, Date.valueOf(disco.getFechaLanzamiento()));
            stmt.setInt(6, disco.getId());
            return stmt.executeUpdate() > 0;
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }

    public Disco obtenerDiscoPorId(int id) {
        String sql = "SELECT * FROM discos WHERE id = ?";
        try (PreparedStatement stmt = conexion.prepareStatement(sql)) {
            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                return new Disco(
                    rs.getInt("id"),
                    rs.getString("titulo"),
                    rs.getString("artista"),
                    rs.getDouble("precio"),
                    rs.getBoolean("disponible"),
                    rs.getDate("fecha_lanzamiento").toLocalDate()
                );
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    public boolean eliminarDisco(int id) {
        String sql = "DELETE FROM discos WHERE id = ?";
        try (PreparedStatement stmt = conexion.prepareStatement(sql)) {
            stmt.setInt(1, id);
            return stmt.executeUpdate() > 0;
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }

    public boolean eliminarDisco(Disco disco) {
        return eliminarDisco(disco.getId());
    }

    public boolean existeDisco(Disco disco) {
        String sql = "SELECT COUNT(*) FROM discos WHERE titulo = ? AND artista = ?";
        try (PreparedStatement stmt = conexion.prepareStatement(sql)) {
            stmt.setString(1, disco.getTitulo());
            stmt.setString(2, disco.getArtista());
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                return rs.getInt(1) > 0;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return false;
    }

    public ArrayList<Disco> obtenerTodosDiscos() {
        ArrayList<Disco> discos = new ArrayList<>();
        String sql = "SELECT * FROM discos";
        try (Statement stmt = conexion.createStatement(); ResultSet rs = stmt.executeQuery(sql)) {
            while (rs.next()) {
                discos.add(new Disco(
                    rs.getInt("id"),
                    rs.getString("titulo"),
                    rs.getString("artista"),
                    rs.getDouble("precio"),
                    rs.getBoolean("disponible"),
                    rs.getDate("fecha_lanzamiento").toLocalDate()
                ));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return discos;
    }

    public int totalDiscosBD() {
        String sql = "SELECT COUNT(*) FROM discos";
        try (Statement stmt = conexion.createStatement(); ResultSet rs = stmt.executeQuery(sql)) {
            if (rs.next()) {
                return rs.getInt(1);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }

    public Disco obtenerDiscoMasCaro() {
        return obtenerDiscoOrdenado("precio", "DESC");
    }

    public Disco obtenerDiscoNovedad() {
        return obtenerDiscoOrdenado("fecha_lanzamiento", "DESC");
    }

    private Disco obtenerDiscoOrdenado(String campo, String orden) {
        String sql = "SELECT * FROM discos ORDER BY " + campo + " " + orden + " LIMIT 1";
        try (Statement stmt = conexion.createStatement(); ResultSet rs = stmt.executeQuery(sql)) {
            if (rs.next()) {
                return new Disco(
                    rs.getInt("id"),
                    rs.getString("titulo"),
                    rs.getString("artista"),
                    rs.getDouble("precio"),
                    rs.getBoolean("disponible"),
                    rs.getDate("fecha_lanzamiento").toLocalDate()
                );
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

***MÉTODOS A PROBAR MAÑANA: *** 
public ArrayList<Disco> obtenerDiscosPorArtistaYPrecio(String artista, double precioMax)
Devuelve los discos de un artista específico cuyo precio no supere un valor dado.

public boolean actualizarDisponibilidadDisco(int id, boolean disponible)
Actualiza la disponibilidad de un disco, dado su ID y el estado de disponibilidad (true/false).

public ArrayList<Disco> obtenerDiscosPorGenero(String genero)
Devuelve una lista de discos filtrados por género.

public ArrayList<Disco> obtenerDiscosPorFechaDeLanzamiento(String fechaLanzamiento)
Devuelve los discos lanzados en una fecha específica o después de esa fecha.

public boolean incrementarPrecioDisco(int id, double incremento)
Incrementa el precio de un disco específico en una cantidad dada.

public Disco obtenerDiscoMasBarato()
Similar al de "más caro", pero devuelve el disco con el precio más bajo.

public int contarDiscosPorArtista(String artista)
Devuelve el número de discos que tiene un determinado artista en la base de datos.

public boolean eliminarDiscosPorArtista(String artista)
Elimina todos los discos asociados con un artista dado.

public ArrayList<Disco> obtenerDiscosPorMesDeLanzamiento(int mes)
Devuelve los discos lanzados en un mes específico.

public boolean existeDiscoPorTitulo(String titulo)
Verifica si existe un disco con el título proporcionado.







