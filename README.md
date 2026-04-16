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

🏗️ Estructura del Proyecto
src/main/java/com/template/projectCleanArquitecture
📦 projectCleanArquitecture
┣ 📂 application
┃ ┗ 📂 usecase
┣ 📂 domain
┃ ┣ 📂 Enums
┃ ┣ 📂 exceptions
┃ ┣ 📂 gateways
┃ ┣ 📂 model
┃ ┣ 📂 ports
┃ ┣ 📂 repository
┃ ┗ 📂 service
┣ 📂 infrastructure
┃ ┣ 📂 config
┃ ┣ 📂 driven_adapters
┃ ┣ 📂 entry_points
┃ ┗ 📂 mappers
┣ 📂 shared
┃ ┗ 📂 utils
┗ 📜 ProjectCleanArquitectureApplication.java


1. 🧠 Domain (Dominio)

Contiene la lógica pura del negocio.

Incluye:
model: entidades del dominio
Enums: constantes del negocio
exceptions: excepciones personalizadas
repository: interfaces de persistencia
ports: contratos de entrada/salida
service: lógica de negocio pura
gateways: comunicación abstracta con externos

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

Implementa los detalles técnicos.

Incluye:
config: configuración de Spring
driven_adapters: acceso a BD, APIs externas
entry_points: controladores REST
mappers: conversión DTO ↔ dominio

Aquí vive todo lo que depende de frameworks:

Spring Boot
JPA / JDBC
APIs externas


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
Infrastructure ➝ Application ➝ Domain

✔ Siempre hacia adentro
✔ Uso de interfaces para desacoplar

Se usan interfaces (puertos) para desacoplar

🧪 Testing
src/test/java/com/template/projectCleanArquitecture
Pruebas unitarias para dominio
Pruebas de casos de uso con mocks
Pruebas de integración en infraestructura

⚙️ Configuración

Archivo principal:

src/main/resources/application.properties

🛠️ Tecnologías
Java 17+
Spring Boot
Maven
JDBC / JPA
PostgreSQL (opcional)

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

🧱 Ejemplo de Flujo
entry_points recibe request
Llama a usecase
usecase usa repository (interfaz)
driven_adapter implementa esa interfaz
Se retorna respuesta


📬 Contribución
Fork del proyecto
Crear rama (feature/nueva-funcionalidad)
Commit
Pull Request

Si deseas mejorar esta plantilla:

Haz un fork
Crea una rama (feature/nueva-mejora)
Haz commit de tus cambios
Abre un Pull Request
📜 Licencia

Este proyecto está bajo la licencia MIT.