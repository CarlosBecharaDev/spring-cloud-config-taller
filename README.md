# Spring Cloud Config Server y Client

> Taller de configuracion centralizada para microservicios usando Spring Cloud

---

## Descripcion

Este proyecto implementa un servidor de configuracion centralizada utilizando **Spring Cloud Config Server** y un cliente denominado **loan-service** que consume dicha configuracion. Permite gestionar multiples perfiles de ambiente (default, dev, uat) de forma desacoplada.

---

## Arquitectura

```
                         ┌─────────────────────────┐
                         │    CONFIG SERVER         │
                         │    Puerto: 8888          │
                         │    Repositorio: Native   │
                         └────────────┬────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
          ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
          │  CLIENT DEFAULT │ │   CLIENT DEV    │ │   CLIENT UAT    │
          │  Puerto: 8080   │ │  Puerto: 8081   │ │  Puerto: 8082   │
          │  /mensaje       │ │  /mensaje       │ │  /mensaje       │
          └─────────────────┘ └─────────────────┘ └─────────────────┘
```

---

## Estructura del Proyecto

```
spring-cloud-config-taller/
│
├── config-repo/                    # Archivos de configuracion
│   ├── loan-service.properties     # Perfil default (puerto 8080)
│   ├── loan-service-dev.properties # Perfil desarrollo (puerto 8081)
│   └── loan-service-uat.properties # Perfil UAT (puerto 8082)
│
├── config-server/                  # Servidor de configuracion
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/example/configserver/
│       │   └── ConfigServerApplication.java
│       └── resources/
│           └── application.properties
│
├── config-client/                  # Cliente loan-service
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/example/loanservice/
│       │   ├── LoanServiceApplication.java
│       │   └── LoanController.java
│       └── resources/
│           └── application.yml
│
├── capturas/                       # Evidencias de pruebas
├── Postman_Collection.json         # Coleccion de Postman
├── Postman_Environment.json        # Environment para Postman
├── INFORME.md                      # Informe detallado
└── README.md
```

---

## Requisitos

- **Java:** 21 o superior
- **Maven:** 3.9.x o superior
- **Postman:** (opcional) Para ejecutar las pruebas automatizadas

---

## Instalacion y Ejecucion

### 1. Clonar el repositorio

```bash
git clone https://github.com/CarlosBecharaDev/spring-cloud-config-taller.git
cd spring-cloud-config-taller
```

### 2. Iniciar el Config Server

```bash
cd config-server
mvn spring-boot:run
```

El servidor iniciara en: `http://localhost:8888`

### 3. Iniciar los Clientes (en terminales separadas)

**Perfil Default (puerto 8080):**
```bash
cd config-client
mvn spring-boot:run "-Dspring-boot.run.arguments=--spring.profiles.active=default"
```

**Perfil Development (puerto 8081):**
```bash
cd config-client
mvn spring-boot:run "-Dspring-boot.run.arguments=--spring.profiles.active=dev"
```

**Perfil UAT (puerto 8082):**
```bash
cd config-client
mvn spring-boot:run "-Dspring-boot.run.arguments=--spring.profiles.active=uat"
```

---

## Pruebas

### Prueba manual con Postman

| Servicio | Perfil | URL | Puerto |
|----------|--------|-----|--------|
| Config Server | default | `GET http://localhost:8888/loan-service/default` | 8888 |
| Config Server | dev | `GET http://localhost:8888/loan-service/dev` | 8888 |
| Config Server | uat | `GET http://localhost:8888/loan-service/uat` | 8888 |
| Client | default | `GET http://localhost:8080/mensaje` | 8080 |
| Client | dev | `GET http://localhost:8081/mensaje` | 8081 |
| Client | uat | `GET http://localhost:8082/mensaje` | 8082 |

---

## Respuestas Esperadas

| Perfil | Puerto | Respuesta |
|--------|--------|-----------|
| default | 8080 | `Bienvenido Desde El Perfil Por Defecto` |
| dev | 8081 | `Bienvenido Desde El Perfil De Desarrollo` |
| uat | 8082 | `Bienvenido Desde El Perfil De UAT` |

---

## Tecnologias

| Tecnologia | Version | Descripcion |
|------------|---------|-------------|
| Java | 21 | Lenguaje de programacion |
| Spring Boot | 3.2.5 | Framework principal |
| Spring Cloud Config Server | 4.1.1 | Servidor de configuracion |
| Spring Cloud Config Client | 4.1.1 | Cliente de configuracion |
| Maven | 3.9.16 | Gestor de dependencias |
| Apache Tomcat | 10.1.20 | Servidor web embebido |
