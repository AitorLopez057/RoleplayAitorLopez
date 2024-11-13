# RoleplayAitorLopez

Este es un proyecto de aplicación web para gestionar criaturas de un juego de rol. La aplicación permite a los usuarios
 ver, crear, editar, y eliminar criaturas a través de una interfaz sencilla. 
 Este proyecto está desarrollado en PHP y utiliza una base de datos PhpMyAdmin, alojado en un entorno XAMPP.

## Requisitos Previos

Para ejecutar este proyecto en tu entorno de desarrollo local, necesitarás:
- [XAMPP](https://www.apachefriends.org/download.html) (incluye Apache, PHP y PhpMyAdmin).
- Conocimientos básicos de PHP, MySQL y programación en MVC.
- Un navegador web para acceder a la interfaz de la aplicación.

# Explicación del Diagrama de Clases

Este proyecto utiliza varias clases para gestionar y manipular datos de personajes (Character) en un sistema de juego de rol. A continuación se describe la función de cada clase en el sistema y sus interacciones.



## Clases Principales

### 1. `Character`
La clase `Character` representa la entidad de un personaje en el sistema. Incluye atributos que definen las propiedades de un personaje, así como métodos para acceder y modificar estas propiedades.

- **Atributos**:
  - `characterid`: Identificador único del personaje.
  - `name`: Nombre del personaje.
  - `description`: Descripción del personaje.
  - `avatar`: URL o referencia a la imagen del personaje.
  - `attackPower`: Poder de ataque del personaje.
  - `lifeLevel`: Nivel de vida del personaje.
  - `weapon`: Arma que posee el personaje.

- **Métodos**:
  - Métodos `get` y `set` para cada atributo (ej. `getName()`, `setAttackPower()`, etc.), que permiten leer y actualizar las propiedades del personaje.
  - `Character::__construct()`: Constructor para inicializar un personaje.
  - `privateCreature2HTML()`: Método privado que genera una representación HTML del personaje, probablemente para mostrarlo en la interfaz.

### 2. `CharacterDAO`
La clase `CharacterDAO` actúa como un objeto de acceso a datos (DAO, Data Access Object) para la clase `Character`. 
Se encarga de las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en la base de datos, gestionando las consultas SQL relacionadas con los personajes.

- **Atributos**:
  - `CHARACTER_TABLE`: Constante que representa el nombre de la tabla en la base de datos donde se almacenan los personajes.

- **Métodos**:
  - `__construct()`: Constructor para inicializar la conexión con la base de datos.
  - `selectAll()`: Recupera todos los personajes de la base de datos.
  - `insert(character)`: Inserta un nuevo personaje en la base de datos.
  - `selectById(id)`: Recupera un personaje específico basado en su ID.
  - `update(character)`: Actualiza la información de un personaje en la base de datos.
  - `delete(id)`: Elimina un personaje de la base de datos.

### 3. `CharacterController`
La clase `CharacterController` es el controlador que maneja las acciones y eventos relacionados con los personajes, 
integrando la lógica de la aplicación con las operaciones de la interfaz de usuario y la capa de acceso a datos.

- **Atributos**:
  - `_characterController`: Instancia del controlador.

- **Métodos**:
  - `__construct()`: Constructor que inicializa el controlador.
  - `readAction()`: Acción para leer y listar los personajes.
  - `index()`: Acción principal que muestra la lista de personajes.
  - `insertCharacter()`: Acción para insertar un nuevo personaje.
  - `editAction()`: Acción para editar un personaje existente.
  - `deleteAction()`: Acción para eliminar un personaje.

### 4. `PersistentManager`
La clase `PersistentManager` gestiona la conexión con la base de datos, proporcionando métodos para establecer 
y cerrar la conexión. Esta clase sigue el patrón singleton, lo que asegura que solo exista una instancia de conexión a la base de datos en todo el sistema.

- **Atributos**:
  - `instance`: Instancia única de la clase.
  - `connection`: Conexión activa a la base de datos.
  - `userBD`, `psswdBD`, `nameBD`, `hostBD`: Información de conexión, como usuario, contraseña, nombre de la base de datos y host.

- **Métodos**:
  - `getInstance()`: Devuelve la instancia única de `PersistentManager`.
  - `establishCredentials()`: Establece las credenciales para la conexión a la base de datos.
  - `close_connection()`: Cierra la conexión con la base de datos.
  - Métodos `get` para obtener los detalles de conexión, como el host, el usuario y la base de datos.

### 5. `ValidationRules`
La clase `ValidationRules` se encarga de validar la entrada de datos antes de que sean procesados o 
almacenados en la base de datos. Esto asegura que los datos ingresados cumplan con los requisitos del sistema.

- **Métodos**:
  - `test_input(data)`: Método para validar y limpiar los datos de entrada.
---

## Diagrama de Interacción

Las clases interactúan de la siguiente manera:

1. **Interfaz de Usuario**: Las acciones del usuario (como crear, editar o eliminar un personaje) se manejan en la capa de interfaz de usuario.
2. **CharacterController**: El controlador recibe las solicitudes de la interfaz y usa `CharacterDAO` para realizar las operaciones necesarias en la base de datos.
3. **CharacterDAO**: Esta clase realiza las consultas SQL, apoyándose en `PersistentManager` para gestionar la conexión con la base de datos.
4. **PersistentManager**: Proporciona la conexión a la base de datos, que `CharacterDAO` utiliza para ejecutar operaciones CRUD.
5. **ValidationRules**: Valida los datos de entrada antes de que `CharacterController` los pase a `CharacterDAO`.

Este diagrama asegura una separación clara entre las capas de presentación, lógica de negocio y acceso a datos, 
promoviendo una arquitectura bien estructurada y modular.