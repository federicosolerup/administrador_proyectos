# Administrador de Proyectos

Aplicación de escritorio desarrollada en **Java + Swing** con base de datos **H2**, diseñada para gestionar empleados, tareas, proyectos y todas sus relaciones. Incluye asignaciones, afiliaciones, historial de trabajo y reportes integrados.

El sistema usa una **arquitectura en capas** (DAO → H2Impl → Service → UI) y un conjunto de clases genéricas que permiten agregar nuevos módulos fácilmente.

---

# Características principales

* Gestión completa de **Empleados**, **Proyectos** y **Tareas**
* **Asignación** de empleados a proyectos
* **Afiliación** de tareas a proyectos
* **Historial** de trabajo con fecha, estado y horas reales
* **Reportes** combinando empleado, proyecto y tarea
* UI construida en Swing con paneles reutilizables
* Base de datos **H2 embebida**, creada automáticamente al iniciar
* Navegación dinámica mediante reflexión (PanelManager)

---

# Arquitectura general

El proyecto sigue una estructura clara y consistente:

```
Entidad → DAO → H2Impl → Service → Panel (UI)
```

Además incluye un framework base con:

* `GenericDAO<T>`
* `GenericH2Impl<T>`
* `GenericService<T>`
* `GenericPanel` (Swing)
* `DBManager` + `DBUtils`
* `PanelManager`
* `TableDefinitions`
* `Validador`
* `MenuPanel`

---

# Flujo de ejecución

1. **Main** arranca el sistema.

   * Recorre todas las tablas definidas en `TableDefinitions`.
   * Ejecuta `CREATE TABLE IF NOT EXISTS` mediante `TableManager`.
   * Inicia la UI con `PanelManager`.

2. **PanelManager** crea la ventana principal y muestra el menú.

3. Al elegir un módulo, PanelManager instancia dinámicamente el panel adecuado:

   * `"Empleado"` → `Empleado.EmpleadoPanel`
   * `"Tarea"` → `Tarea.TareaPanel`
   * etc.

4. Cada panel usa su **Service** → **DAO** → **H2Impl** para leer/escribir en la base.

---

# Base de Datos

El sistema usa **H2 en modo file**, con URL:

```
jdbc:h2:file:./db/DB
Usuario: sa
Password: (vacío)
```

Incluye todas las tablas necesarias para el proyecto:

* Empleado
* Proyecto
* Tarea
* Asignacion
* Afiliacion
* Historial
* (Reporte usa consultas combinadas)

La consola H2 puede iniciarse con:

```bash
sh h2.sh
```

---

# Módulos funcionales

## **Empleado**

Gestiona nombre, apellido y costo por hora.
Incluye CRUD completo y validación numérica.

## **Proyecto**

Registra proyectos con su descripción.
CRUD completo.

## **Tarea**

Define título, descripción y horas estimadas.
CRUD y validación.

## **Asignación** (Empleado ↔ Proyecto)

Permite asignar empleados a un proyecto.
Evita duplicados filtrando empleados ya asignados.

## **Afiliación** (Tarea ↔ Proyecto)

Vincula tareas con proyectos.
Filtra tareas ya asociadas al proyecto seleccionado.

## **Historial** (Tarea ↔ Empleado)

Registra:

* Fecha del ajuste
* Estado: *EN CURSO* o *FINALIZADA*
* Horas reales (obligatorias al finalizar)

Vincula tarea y empleado, y lista todo en tabla.

## **Reportes**

Genera consultas que combinan:

* Proyecto
* Empleado
* Tarea
* Horas estimadas
* Horas reales ejecutadas

Contenido solo de lectura.

---

# Framework interno

### **GenericDAO<T>**

Contrato base del CRUD.

### **GenericH2Impl<T>**

Implementación genérica para H2:
inserción, actualización, borrado, lectura por ID, lectura total, mapeo automático.

### **GenericService<T>**

Capa intermedia que maneja excepciones y conecta UI ↔ DAO.

### **GenericPanel**

Base para todos los paneles Swing:
tabla, botones CRUD, inputPanel, hooks abstractos.

### **PanelManager**

Ventana principal, crea paneles por reflexión y los almacena para reuso.

### **MenuPanel**

Pantalla principal con las opciones del sistema.

### **TableDefinitions**

Todas las sentencias SQL de creación de tablas + nombres de tablas.

### **Validador**

Funciones de validación numérica y de entrada.

---

# Cómo ejecutar el proyecto

1. Instalar **JDK 8 o superior**.
2. Clonar este repositorio.
3. Abrirlo en tu IDE (Eclipse, IntelliJ, NetBeans).
4. Confirmar que el archivo **h2-2.2.224.jar** esté en el classpath.
5. Ejecutar la clase:

```
Main.Main
```

6. Usar el menú gráfico para navegar entre los módulos.

---

# Estructura del proyecto (resumen)

```
/Empleado
/Proyecto
/Tarea
/Asignacion
/Afiliacion
/Historial
/Reporte

/Main
/DB
/Utilidades
```

---

# Estado del proyecto

Proyecto académico/ejemplo de arquitectura en capas con persistencia H2 y UI Swing.
Modular, extensible y listo para ampliarse con nuevos paneles o entidades siguiendo la misma estructura.
