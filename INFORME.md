# INFORME - Spring Cloud Config Server y Client

## 1. Objetivo
Implementar un servidor de configuracion centralizada usando Spring Cloud Config Server y un cliente (loan-service) que consuma dicha configuracion, demostrando la gestion de perfiles (default, dev, uat) en una arquitectura de microservicios.

## 2. Tecnologias
- Java 21
- Spring Boot 3.2.5
- Spring Cloud Config Server 4.1.1
- Spring Cloud Config Client 4.1.1
- Maven 3.9.16

## 3. Arquitectura

```
┌─────────────────────┐         ┌──────────────────────────┐
│   CONFIG CLIENT      │         │    CONFIG SERVER          │
│   (loan-service)     │ ──────> │    Puerto: 8888           │
│                      │         │    Repositorio: Native    │
│   Perfil Default:    │         │    (config-repo/)         │
│     Puerto 8080      │         │                           │
│   Perfil Dev:        │         │    Archivos:              │
│     Puerto 8081      │         │    loan-service.properties│
│   Perfil UAT:        │         │    loan-service-dev.*     │
│     Puerto 8082      │         │    loan-service-uat.*     │
└─────────────────────┘         └──────────────────────────┘
```

## 4. Estructura del Proyecto

```
spring-cloud-electiva/
├── config-repo/
│   ├── loan-service.properties
│   ├── loan-service-dev.properties
│   └── loan-service-uat.properties
├── config-server/
│   ├── pom.xml
│   └── src/main/java/.../ConfigServerApplication.java
├── config-client/
│   ├── pom.xml
│   └── src/main/java/.../LoanServiceApplication.java
│   └── src/main/java/.../LoanController.java
├── capturas/
├── README.md
└── INFORME.md
```

## 5. Codigos Fuente

### 5.1 ConfigServerApplication.java
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

### 5.2 application.properties (Config Server)
```properties
spring.application.name=config-server
server.port=8888
spring.profiles.active=native
spring.cloud.config.server.native.search-locations=file:///${user.dir}/../config-repo
```

### 5.3 LoanController.java
```java
@RestController
public class LoanController {
    @Value("${application.message}")
    private String message;

    @GetMapping("/mensaje")
    public String getMessage() {
        return message;
    }
}
```

### 5.4 application.yml (Config Client)
```yaml
spring:
  application:
    name: loan-service
  config:
    import: optional:configserver:http://localhost:8888
  profiles:
    active: dev
```

## 6. Evidencias

### 6.1 Config Server - Perfil Dev
**URL:** GET http://localhost:8888/loan-service/dev
**Respuesta:**
```json
{
  "name": "loan-service",
  "profiles": ["dev"],
  "propertySources": [{
    "name": "loan-service-dev.properties",
    "source": {
      "server.port": "8081",
      "application.message": "Welcome From Development Profile"
    }
  }]
}
```
![Config Server Dev](capturas/01-servidor-dev.png)

### 6.2 Config Client - Perfil Dev
**URL:** GET http://localhost:8081/mensaje
**Respuesta:** `Welcome From Development Profile`
![Cliente Dev](capturas/02-consola-cliente-dev.png)
![Mensaje Dev](capturas/03-mensaje-dev.png)

### 6.3 Config Client - Perfil UAT
**URL:** GET http://localhost:8082/mensaje
**Respuesta:** `Welcome From UAT Profile`
![Mensaje UAT](capturas/04-mensaje-uat.png)

### 6.4 Config Client - Perfil Default
**URL:** GET http://localhost:8080/mensaje
**Respuesta:** `Welcome From Default Profile`
![Mensaje Default](capturas/05-mensaje-default.png)

## 7. Resultados

| # | Prueba | URL | Puerto | Resultado |
|---|--------|-----|--------|-----------|
| 1 | Config Server - Default | GET /loan-service/default | 8888 | PASS |
| 2 | Config Server - Dev | GET /loan-service/dev | 8888 | PASS |
| 3 | Config Server - UAT | GET /loan-service/uat | 8888 | PASS |
| 4 | Client Default | GET /mensaje | 8080 | PASS |
| 5 | Client Dev | GET /mensaje | 8081 | PASS |
| 6 | Client UAT | GET /mensaje | 8082 | PASS |

## 8. Conclusiones
1. **Configuracion Centralizada:** Spring Cloud Config Server permite administrar todas las configuraciones en un solo lugar.
2. **Gestion de Perfiles:** Se implementaron 3 perfiles (default, dev, uat) con propiedades diferentes.
3. **Desacoplamiento:** El cliente obtiene su configuracion del servidor al iniciar.
4. **Facilidad de Cambio:** Cambiar de perfil solo requiere modificar una linea y reiniciar.
5. **Escalabilidad:** Patron fundamental en arquitecturas de microservicios.

---
**Fecha:** 20 de Septiembre de 2026