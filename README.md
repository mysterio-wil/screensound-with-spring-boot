# Documentación del Proyecto: ScreenSound

## 1. Descripción del Proyecto

**ScreenSound** es una aplicación de consola desarrollada en Java utilizando Spring Boot, diseñada para gestionar artistas y sus canciones. La aplicación permite registrar artistas y canciones, buscar canciones por artistas, y obtener detalles sobre los artistas y su discografía.

### Tecnologías Utilizadas:
- **Java 17 o superior**
- **Spring Boot** (v2.5.5)
- **JPA (Java Persistence API)**
- **PostgreSQL** (Base de datos)
- **Hibernate**
- **Maven** (Gestión de dependencias)

### Objetivo:
El proyecto tiene como objetivo crear una aplicación de gestión musical que permita:
- Registrar y listar artistas y sus canciones.
- Buscar canciones por nombre de artista.
- Obtener detalles sobre los artistas y sus discos.

---

## 2. Estructura del Proyecto

### 2.1. Paquetes

1. **`com.alura.screensound.model`**: Contiene las clases de dominio, que representan los datos de la aplicación.
   - **Artista.java**: Representa a un artista musical.
   - **Cancion.java**: Representa una canción asociada a un artista.
   - **TipoArtista.java**: Enum que define los tipos de artistas (SOLO, DUO, BANDA).

2. **`com.alura.screensound.repository`**: Contiene los repositorios que interactúan con la base de datos a través de JPA.
   - **ArtistaRepository.java**: Repositorio que proporciona métodos para interactuar con la entidad `Artista` y sus relaciones.

3. **`com.alura.screensound.principal`**: Contiene la lógica de la interfaz de usuario y el flujo principal de la aplicación.
   - **Principal.java**: Contiene la lógica que maneja las opciones del menú de la aplicación y la interacción con el repositorio de artistas.

4. **`com.alura.screensound`**: Contiene la clase principal para arrancar la aplicación.
   - **ScreensoundApplication.java**: Configuración y arranque de la aplicación Spring Boot.

---

## 3. Diagrama de Clases

### 3.1. Artista
```java
@Entity
@Table(name = "artistas")
public class Artista {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true)
    private String nombre;
    
    @Enumerated(EnumType.STRING)
    private TipoArtista tipo;
    
    @OneToMany(mappedBy = "artista", cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    private List<Cancion> canciones = new ArrayList<>();
}
```
- **Atributos:**
   - `id`: Identificador único del artista.
   - `nombre`: Nombre del artista.
   - `tipo`: Tipo de artista (SOLO, DUO, BANDA).
   - `canciones`: Lista de canciones asociadas al artista.

- **Relaciones:**
   - Un artista puede tener varias canciones (relación `@OneToMany`).

---

### 3.2. Cancion
```java
@Entity
@Table(name = "musicas")
public class Cancion {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String titulo;
    
    @ManyToOne
    private Artista artista;
}
```
- **Atributos:**
   - `id`: Identificador único de la canción.
   - `titulo`: Título de la canción.
   - `artista`: Artista al que pertenece la canción (relación `@ManyToOne`).

---

### 3.3. TipoArtista (Enum)
```java
public enum TipoArtista {
    SOLO,
    DUO,
    BANDA;
}
```
- Enum que define los tipos de artistas posibles.

---

## 4. Flujo de la Aplicación

### 4.1. Menú Principal

La clase `Principal` maneja el menú interactivo de la aplicación y permite al usuario realizar las siguientes acciones:

- **Registrar artistas**: Permite ingresar el nombre y el tipo de un artista.
- **Registrar canciones**: Permite agregar canciones a los artistas ya registrados.
- **Listar canciones**: Muestra todas las canciones asociadas a los artistas.
- **Buscar canciones por artista**: Permite buscar canciones asociadas a un artista por su nombre.
- **Buscar datos sobre un artista**: Permite buscar y ver los detalles (nombre, tipo y canciones) de un artista.

---

### 4.2. Flujo de Registro de Artistas y Canciones

1. **Registrar Artista**:
   - El usuario ingresa el nombre del artista y el tipo (SOLO, DUO, BANDA).
   - El artista es guardado en la base de datos.

2. **Registrar Canción**:
   - El usuario ingresa el nombre de un artista ya registrado.
   - Luego se ingresa el título de la canción que se desea asociar al artista.
   - La canción se guarda en la base de datos y se asocia al artista.

---

### 4.3. Flujo de Búsqueda

1. **Buscar Canciones por Artista**:
   - El usuario ingresa el nombre de un artista y se muestran todas las canciones asociadas a ese artista.

2. **Buscar Datos del Artista**:
   - El usuario ingresa el nombre de un artista.
   - Si el artista es encontrado, se muestran sus detalles junto con sus canciones.

---

## 5. Base de Datos

### 5.1. Configuración de PostgreSQL

En el archivo `application.properties`, se configura la conexión con la base de datos PostgreSQL:

```properties
spring.datasource.url=jdbc:postgresql://${DB_HOST}/screensound_db
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
spring.datasource.driver-class-name=org.postgresql.Driver
hibernate.dialect=org.hibernate.dialect.HSQLDialect
spring.jpa.hibernate.ddl-auto=update
```
- `DB_HOST`, `DB_USER`, y `DB_PASSWORD` deben ser configurados en las variables de entorno para establecer una conexión adecuada a la base de datos.

---

## 6. Instalación y Ejecución

### 6.1. Requisitos previos
- **Java 17** o superior.
- **PostgreSQL** instalado y en ejecución.
- **Maven** para la gestión de dependencias.

### 6.2. Pasos para ejecutar el proyecto

1. Clona el repositorio:
   ```bash
   git clone <URL del repositorio>
   ```

2. Accede al directorio del proyecto:
   ```bash
   cd screensound
   ```

3. Instala las dependencias utilizando Maven:
   ```bash
   mvn clean install
   ```

4. Configura las variables de entorno para PostgreSQL (`DB_HOST`, `DB_USER`, `DB_PASSWORD`).

5. Ejecuta la aplicación:
   ```bash
   mvn spring-boot:run
   ```

6. Sigue las instrucciones en la consola para interactuar con la aplicación.

---

## 7. Conclusión

El proyecto **ScreenSound** es una aplicación sencilla pero efectiva para gestionar artistas y canciones. Utiliza Spring Boot para el desarrollo backend y JPA/Hibernate para interactuar con la base de datos PostgreSQL. Los usuarios pueden registrar artistas, canciones y realizar consultas interactivas.