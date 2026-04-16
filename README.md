# TemplateProyectCleanArquitecture
Proyecto para ser usado como plantilla para manejo de arquitectura limpia

🧩 Clean Architecture Template

Plantilla base para proyectos backend siguiendo los principios de Clean Architecture, enfocada en separación de responsabilidades, escalabilidad y mantenibilidad.

🚀 Objetivo

Proveer una estructura clara y reutilizable que permita:

Separar la lógica de negocio de frameworks externos
Facilitar pruebas unitarias
Escalar el sistema sin acoplamientos innecesarios
Aplicar principios SOLID
🏗️ Arquitectura

El proyecto está dividido en tres capas principales:

📦 project-root
 ┣ 📂 domain
 ┣ 📂 usecase
 ┣ 📂 infrastructure
 ┗ 📂 application (opcional - entrypoints/config)
1. 🧠 Domain (Dominio)

Contiene la lógica pura del negocio.

Entidades
Value Objects
Reglas de negocio
Interfaces (puertos)

❌ No depende de ninguna otra capa
✅ Es el núcleo del sistema

Ejemplo:

public class User {
    private String id;
    private String name;

    // lógica de negocio
}
2. ⚙️ UseCase (Aplicación)

Orquesta los casos de uso del sistema.

Casos de uso (interactors)
DTOs
Interfaces de entrada/salida

Depende solo de domain

Ejemplo:

public class CreateUserUseCase {

    private final UserRepository repository;

    public CreateUserUseCase(UserRepository repository) {
        this.repository = repository;
    }

    public void execute(User user) {
        repository.save(user);
    }
}
3. 🔌 Infrastructure (Infraestructura)

Implementaciones técnicas y acceso a servicios externos.

Base de datos (JPA, JDBC, etc.)
APIs externas
Configuración de Spring
Adaptadores

Depende de usecase y domain

Ejemplo:

@Repository
public class UserRepositoryImpl implements UserRepository {

    @Override
    public void save(User user) {
        // implementación con JPA o JDBC
    }
}
4. 🌐 Application (Opcional)

Punto de entrada del sistema.

Controladores (REST)
Configuración
Seguridad

Ejemplo:

@RestController
@RequestMapping("/users")
public class UserController {

    private final CreateUserUseCase useCase;

    @PostMapping
    public ResponseEntity<Void> create(@RequestBody User user) {
        useCase.execute(user);
        return ResponseEntity.ok().build();
    }
}
🔄 Flujo de Dependencias
Infrastructure ➝ UseCase ➝ Domain
Las dependencias siempre apuntan hacia el dominio
Se usan interfaces (puertos) para desacoplar
🧪 Testing
domain: pruebas unitarias puras
usecase: pruebas con mocks
infrastructure: pruebas de integración
📦 Tecnologías sugeridas
Java 17+
Spring Boot
Maven / Gradle
PostgreSQL / MySQL
JUnit + Mockito
📌 Principios aplicados
SOLID
Inversión de dependencias
Separación de responsabilidades
Arquitectura hexagonal (inspiración)
🛠️ Cómo usar esta plantilla
Clonar el repositorio
Renombrar paquetes según tu proyecto
Definir tus entidades en domain
Crear casos de uso en usecase
Implementar adaptadores en infrastructure
Exponer endpoints en application
📄 Ejemplo de estructura real
com.example.project
 ┣ 📂 domain
 ┃ ┣ 📂 model
 ┃ ┗ 📂 repository
 ┣ 📂 usecase
 ┃ ┗ 📂 user
 ┣ 📂 infrastructure
 ┃ ┣ 📂 persistence
 ┃ ┗ 📂 config
 ┗ 📂 application
   ┗ 📂 controller
📬 Contribuciones

Si deseas mejorar esta plantilla:

Haz un fork
Crea una rama (feature/nueva-mejora)
Haz commit de tus cambios
Abre un Pull Request
📜 Licencia

Este proyecto está bajo la licencia MIT.