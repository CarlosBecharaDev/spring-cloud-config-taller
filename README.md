# Spring Cloud Config - Taller Electiva I

Taller de configuracion centralizada con Spring Cloud: un **Config Server** que sirve las propiedades y un **Config Client** (`loan-service`) que las consume.

## Estructura

- `config-server/`: servidor de configuracion (puerto 8888)
- `config-client/`: cliente `loan-service`
- `config-repo/`: archivos `.properties` que sirve el servidor

## Ejecucion

Requisitos: Java 21 y Maven.

1. Iniciar el servidor:

```bash
cd config-server
mvn spring-boot:run
```

2. En otra terminal, iniciar el cliente:

```bash
cd config-client
mvn spring-boot:run
```

Para probar otro perfil (`dev`, `uat` o `default`):

```bash
mvn spring-boot:run "-Dspring-boot.run.arguments=--spring.profiles.active=uat"
```

Abrir `http://localhost:XXXX/mensaje` (puerto 8080 para `default`, 8081 para `dev`, 8082 para `uat`).

## Evidencias

Ver la carpeta `capturas/` y el archivo `INFORME.md`.