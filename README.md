/**
 * Este archivo contiene todas las clases necesarias para el modelo de datos y el controlador
 * de la aplicación de gestión de la feria empresarial.
 *
 * Estructura de paquetes recomendada:
 * - com.feria.modelo: Clases de datos (Empresa, Stand, Visitante, Comentario)
 * - com.feria.controlador: Lógica de negocio (FeriaControlador)
 * - com.feria.vista: Interfaz de usuario (a crear en NetBeans)
 * - com.feria.main: Clase principal (MainApp)
 */

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;
import java.text.SimpleDateFormat;

// ==============================================================================
// 1. CLASES DEL MODELO DE DATOS (com.feria.modelo)
// ==============================================================================

/**
 * Clase que representa a una Empresa participante en la feria.
 */
public class Empresa {
    private int id;
    private String nombre;
    private String sector;
    private String correoElectronico;
    private Stand standAsignado;

    // Generador de ID automático y único para cada empresa
    private static final AtomicInteger contadorId = new AtomicInteger(0);

    public Empresa(String nombre, String sector, String correoElectronico) {
        this.id = contadorId.incrementAndGet();
        this.nombre = nombre;
        this.sector = sector;
        this.correoElectronico = correoElectronico;
    }

    // Getters y Setters
    public int getId() { return id; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getSector() { return sector; }
    public void setSector(String sector) { this.sector = sector; }
    public String getCorreoElectronico() { return correoElectronico; }
    public void setCorreoElectronico(String correoElectronico) { this.correoElectronico = correoElectronico; }
    public Stand getStandAsignado() { return standAsignado; }
    public void setStandAsignado(Stand standAsignado) { this.standAsignado = standAsignado; }

    @Override
    public String toString() {
        return "Empresa ID: " + id + ", Nombre: " + nombre + ", Sector: " + sector;
    }
}

/**
 * Clase que representa un Stand en la feria.
 */
public class Stand {
    private int numero;
    private String ubicacion;
    private String tamaño;
    private boolean ocupado;
    private Empresa empresaAsignada;
    private List<Comentario> comentarios;

    // Generador de ID automático y único para cada stand
    private static final AtomicInteger contadorNumero = new AtomicInteger(0);

    public Stand(String ubicacion, String tamaño) {
        this.numero = contadorNumero.incrementAndGet();
        this.ubicacion = ubicacion;
        this.tamaño = tamaño;
        this.ocupado = false;
        this.comentarios = new ArrayList<>();
    }

    // Getters y Setters
    public int getNumero() { return numero; }
    public String getUbicacion() { return ubicacion; }
    public void setUbicacion(String ubicacion) { this.ubicacion = ubicacion; }
    public String getTamaño() { return tamaño; }
    public void setTamaño(String tamaño) { this.tamaño = tamaño; }
    public boolean isOcupado() { return ocupado; }
    public void setOcupado(boolean ocupado) { this.ocupado = ocupado; }
    public Empresa getEmpresaAsignada() { return empresaAsignada; }
    public void setEmpresaAsignada(Empresa empresaAsignada) { this.empresaAsignada = empresaAsignada; }
    public List<Comentario> getComentarios() { return comentarios; }
    public void agregarComentario(Comentario comentario) { this.comentarios.add(comentario); }

    @Override
    public String toString() {
        return "Stand Nº: " + numero + ", Ubicación: " + ubicacion + ", Tamaño: " + tamaño + (ocupado ? " (Ocupado)" : " (Disponible)");
    }
}

/**
 * Clase que representa a un Visitante de la feria.
 */
public class Visitante {
    private int id;
    private String nombre;
    private String numeroIdentificacion;
    private String correoElectronico;
    private List<Stand> standsVisitados; // Para el reporte de visitantes

    // Generador de ID automático y único para cada visitante
    private static final AtomicInteger contadorId = new AtomicInteger(0);

    public Visitante(String nombre, String numeroIdentificacion, String correoElectronico) {
        this.id = contadorId.incrementAndGet();
        this.nombre = nombre;
        this.numeroIdentificacion = numeroIdentificacion;
        this.correoElectronico = correoElectronico;
        this.standsVisitados = new ArrayList<>();
    }

    // Getters y Setters
    public int getId() { return id; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getNumeroIdentificacion() { return numeroIdentificacion; }
    public void setNumeroIdentificacion(String numeroIdentificacion) { this.numeroIdentificacion = numeroIdentificacion; }
    public String getCorreoElectronico() { return correoElectronico; }
    public void setCorreoElectronico(String correoElectronico) { this.correoElectronico = correoElectronico; }
    public List<Stand> getStandsVisitados() { return standsVisitados; }
    public void visitarStand(Stand stand) {
        if (!standsVisitados.contains(stand)) {
            standsVisitados.add(stand);
        }
    }

    @Override
    public String toString() {
        return "Visitante ID: " + id + ", Nombre: " + nombre + ", Identificación: " + numeroIdentificacion;
    }
}

/**
 * Clase que representa un Comentario dejado por un visitante en un stand.
 */
public class Comentario {
    private Visitante visitante;
    private Date fechaVisita;
    private int calificacion; // 1 a 5 estrellas
    private String textoComentario;

    public Comentario(Visitante visitante, int calificacion, String textoComentario) {
        this.visitante = visitante;
        this.fechaVisita = new Date(); // Fecha actual
        this.calificacion = calificacion;
        this.textoComentario = textoComentario;
    }

    // Getters
    public Visitante getVisitante() { return visitante; }
    public Date getFechaVisita() { return fechaVisita; }
    public int getCalificacion() { return calificacion; }
    public String getTextoComentario() { return textoComentario; }

    @Override
    public String toString() {
        SimpleDateFormat formatter = new SimpleDateFormat("dd/MM/yyyy HH:mm:ss");
        return "Visitante: " + visitante.getNombre() + ", Calificación: " + calificacion + " estrellas, Fecha: " + formatter.format(fechaVisita) + ", Comentario: " + textoComentario;
    }
}


// ==============================================================================
// 2. CLASE DEL CONTROLADOR (com.feria.controlador)
// ==============================================================================

/**
 * Clase principal que actúa como el controlador de la aplicación.
 * Contiene la lógica de negocio para gestionar empresas, stands, visitantes y reportes.
 */
public class FeriaControlador {
    private List<Empresa> empresas;
    private List<Stand> stands;
    private List<Visitante> visitantes;

    public FeriaControlador() {
        this.empresas = new ArrayList<>();
        this.stands = new ArrayList<>();
        this.visitantes = new ArrayList<>();
        // Inicialización de stands para la feria
        inicializarStands();
    }

    private void inicializarStands() {
        stands.add(new Stand("Pabellón A, Stand 1", "Pequeño"));
        stands.add(new Stand("Pabellón A, Stand 2", "Mediano"));
        stands.add(new Stand("Pabellón B, Stand 1", "Grande"));
        stands.add(new Stand("Pabellón B, Stand 2", "Pequeño"));
    }

    // Métodos para la gestión de Empresas
    public void agregarEmpresa(Empresa empresa) {
        empresas.add(empresa);
    }
    public void editarEmpresa(int id, String nombre, String sector, String correo) {
        for (Empresa e : empresas) {
            if (e.getId() == id) {
                e.setNombre(nombre);
                e.setSector(sector);
                e.setCorreoElectronico(correo);
                return;
            }
        }
    }
    public void eliminarEmpresa(int id) {
        empresas.removeIf(e -> e.getId() == id);
    }
    public List<Empresa> getEmpresas() {
        return empresas;
    }
    public Empresa buscarEmpresaPorId(int id) {
        for (Empresa e : empresas) {
            if (e.getId() == id) {
                return e;
            }
        }
        return null;
    }

    // Métodos para la gestión de Stands
    public List<Stand> getStands() {
        return stands;
    }
    public List<Stand> getStandsDisponibles() {
        List<Stand> disponibles = new ArrayList<>();
        for (Stand s : stands) {
            if (!s.isOcupado()) {
                disponibles.add(s);
            }
        }
        return disponibles;
    }
    public List<Stand> getStandsOcupados() {
        List<Stand> ocupados = new ArrayList<>();
        for (Stand s : stands) {
            if (s.isOcupado()) {
                ocupados.add(s);
            }
        }
        return ocupados;
    }
    public Stand buscarStandPorNumero(int numero) {
        for (Stand s : stands) {
            if (s.getNumero() == numero) {
                return s;
            }
        }
        return null;
    }

    // Métodos para la asignación de Stands
    public boolean asignarStand(Empresa empresa, Stand stand) {
        if (!stand.isOcupado()) {
            stand.setEmpresaAsignada(empresa);
            stand.setOcupado(true);
            empresa.setStandAsignado(stand);
            return true;
        }
        return false;
    }

    // Métodos para la gestión de Visitantes
    public void agregarVisitante(Visitante visitante) {
        visitantes.add(visitante);
    }
    public void editarVisitante(int id, String nombre, String identificacion, String correo) {
        for (Visitante v : visitantes) {
            if (v.getId() == id) {
                v.setNombre(nombre);
                v.setNumeroIdentificacion(identificacion);
                v.setCorreoElectronico(correo);
                return;
            }
        }
    }
    public void eliminarVisitante(int id) {
        visitantes.removeIf(v -> v.getId() == id);
    }
    public List<Visitante> getVisitantes() {
        return visitantes;
    }
    public Visitante buscarVisitantePorId(int id) {
        for (Visitante v : visitantes) {
            if (v.getId() == id) {
                return v;
            }
        }
        return null;
    }

    // Métodos para la Interacción
    public boolean dejarComentarioEnStand(int idVisitante, int numeroStand, int calificacion, String texto) {
        Visitante visitante = buscarVisitantePorId(idVisitante);
        Stand stand = buscarStandPorNumero(numeroStand);
        if (visitante != null && stand != null) {
            Comentario comentario = new Comentario(visitante, calificacion, texto);
            stand.agregarComentario(comentario);
            visitante.visitarStand(stand);
            return true;
        }
        return false;
    }

    // Métodos para los Reportes
    public String generarReporteEmpresasStands() {
        StringBuilder sb = new StringBuilder();
        sb.append("--- Reporte de Empresas y Stands Asignados ---\n");
        for (Empresa e : empresas) {
            String standInfo = (e.getStandAsignado() != null) ? e.getStandAsignado().toString() : "No asignado";
            sb.append("Empresa: ").append(e.getNombre()).append(", Sector: ").append(e.getSector()).append("\n");
            sb.append("   Stand Asignado: ").append(standInfo).append("\n");
        }
        return sb.toString();
    }

    public String generarReporteVisitantesYStands() {
        StringBuilder sb = new StringBuilder();
        sb.append("--- Reporte de Visitantes y Stands que han Visitado ---\n");
        for (Visitante v : visitantes) {
            sb.append("Visitante: ").append(v.getNombre()).append("\n");
            sb.append("   Stands Visitados:\n");
            if (v.getStandsVisitados().isEmpty()) {
                sb.append("       Ninguno.\n");
            } else {
                for (Stand s : v.getStandsVisitados()) {
                    sb.append("       - ").append(s.getUbicacion()).append("\n");
                }
            }
        }
        return sb.toString();
    }

    public String generarReporteCalificacionPromedio() {
        StringBuilder sb = new StringBuilder();
        sb.append("--- Reporte de Calificación Promedio por Stand ---\n");
        for (Stand s : stands) {
            double promedio = 0.0;
            if (!s.getComentarios().isEmpty()) {
                int totalCalificaciones = 0;
                for (Comentario c : s.getComentarios()) {
                    totalCalificaciones += c.getCalificacion();
                }
                promedio = (double) totalCalificaciones / s.getComentarios().size();
            }
            sb.append("Stand ").append(s.getUbicacion()).append(": ");
            if (promedio > 0) {
                sb.append(String.format("%.2f", promedio)).append(" estrellas\n");
            } else {
                sb.append("Sin calificaciones.\n");
            }
        }
        return sb.toString();
    }
}

// ==============================================================================
// 3. CLASE PRINCIPAL (com.feria.main) - Ejemplo de uso
// ==============================================================================

/**
 * Clase Main para inicializar y probar el controlador.
 * Esta clase será el punto de entrada de la aplicación.
 */
public class MainApp {
    public static void main(String[] args) {
        FeriaControlador controlador = new FeriaControlador();

        // Ejemplo de registro de empresas y visitantes
        Empresa e1 = new Empresa("Tech Innovators", "Tecnología", "contacto@tech.com");
        Empresa e2 = new Empresa("Health Solutions", "Salud", "info@health.com");
        controlador.agregarEmpresa(e1);
        controlador.agregarEmpresa(e2);

        Visitante v1 = new Visitante("Juan Pérez", "12345678", "juan@correo.com");
        Visitante v2 = new Visitante("Ana Gómez", "87654321", "ana@correo.com");
        controlador.agregarVisitante(v1);
        controlador.agregarVisitante(v2);

        // Ejemplo de asignación de stands
        Stand stand1 = controlador.buscarStandPorNumero(1);
        Stand stand2 = controlador.buscarStandPorNumero(2);
        if (stand1 != null && stand2 != null) {
            controlador.asignarStand(e1, stand1);
            controlador.asignarStand(e2, stand2);
        }

        // Ejemplo de interacción
        controlador.dejarComentarioEnStand(v1.getId(), stand1.getNumero(), 5, "Excelente tecnología!");
        controlador.dejarComentarioEnStand(v2.getId(), stand1.getNumero(), 4, "Muy buen servicio.");
        controlador.dejarComentarioEnStand(v2.getId(), stand2.getNumero(), 5, "Innovador y útil.");

        // Ejemplo de reportes
        System.out.println(controlador.generarReporteEmpresasStands());
        System.out.println(controlador.generarReporteVisitantesYStands());
        System.out.println(controlador.generarReporteCalificacionPromedio());
    }
}
