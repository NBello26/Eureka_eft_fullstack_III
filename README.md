# 🧭 Eureka Server - Sistema de Gestión de Mascotas

Este repositorio contiene el **Servidor de Descubrimiento (Service Registry)** basado en **Spring Cloud Netflix Eureka**. Es una pieza fundamental de la arquitectura de microservicios, encargada de mantener un registro dinámico y actualizado de todas las instancias (y sus respectivas IPs privadas en AWS) que componen el ecosistema.

## 🚀 Arquitectura y Prácticas DevOps

En un entorno Cloud como AWS, las direcciones IP de las instancias EC2 o los contenedores Docker pueden cambiar de forma dinámica tras cada despliegue o reinicio. Eureka soluciona este problema permitiendo que los microservicios se registren por **nombre** en lugar de por IP.

El despliegue está completamente automatizado a través de un pipeline de **CI/CD** con GitHub Actions:
1. Compila el código fuente de Java/Spring Boot.
2. Construye la imagen Docker del servidor Eureka.
3. Sube la imagen a **Amazon Elastic Container Registry (ECR)**.
4. Ejecuta comandos vía **AWS Systems Manager (SSM)** para desplegar el contenedor en su instancia EC2, aislando el tráfico de descubrimiento exclusivamente a la red privada (VPC).

### 🌐 Ecosistema de Infraestructura en AWS
El Servidor Eureka centraliza la comunicación interna de la arquitectura alojada en AWS:

* **Nodo Web:** ApiGateway y Frontend
* **Nodo Back 1:** **Eureka Server** (Este repositorio) 📍 y BFF
* **Nodo Back 2:** Microservicios de Mascotas y Usuarios
* **Nodo Back 3:** Microservicios de Geolocalización y Notificaciones
* **Nodo Back 4:** Motor de Coincidencias
* **Base de Datos:** SanosDB (PostgreSQL)

## 🛠️ Tecnologías Principales

* **Framework:** Java 17 / Spring Boot 3
* **Cloud Native:** Spring Cloud Netflix Eureka Server
* **Contenedores:** Docker
* **CI/CD:** GitHub Actions
* **Infraestructura AWS:** EC2, ECR, SSM, IAM

## ⚙️ Registro y Descubrimiento Dinámico

* **Registro (Self-Registration):** Al iniciar, cada microservicio de negocio (Mascotas, Usuarios, etc.) y las puertas de enlace (BFF, ApiGateway) se conectan a Eureka y registran su IP privada (ej. la IP de su EC2) usando la variable de entorno `$EC2_IP` configurada en los pipelines.
* **Descubrimiento (Discovery):** Cuando un servicio necesita hablar con otro (por ejemplo, el BFF necesita datos de Mascotas), consulta a Eureka por el nombre del servicio (`MS-GESTION-MASCOTAS`). Eureka devuelve la IP actual, garantizando una comunicación ininterrumpida sin depender de IPs estáticas, lo que prepara la infraestructura para auto-escalado horizontal.

## 📦 Repositorios del Proyecto

Explora el resto de la infraestructura y microservicios de este ecosistema:

**Frontend y Puertas de Enlace**
* 🌐 [Frontend_eft_fullstack_III](https://github.com/NBello26/Frontend_eft_fullstack_III)
* 🚪 [ApiGateway_eft_fullstack_III](https://github.com/NBello26/ApiGateway_eft_fullstack_III)
* 🌉 [BFF_eft_fullstack_III](https://github.com/NBello26/BFF_eft_fullstack_III)

**Descubrimiento y Base de Datos**
* 🧭 [Eureka_eft_fullstack_III](https://github.com/NBello26/Eureka_eft_fullstack_III) *(Estás aquí)*
* 🗄️ [BD_eft_fullstack_III](https://github.com/NBello26/BD_eft_fullstack_III)

**Microservicios de Negocio**
* 🐾 [Reporte_Mascota_eft_fullstack_III](https://github.com/NBello26/Reporte_Mascota_eft_fullstack_III)
* 👤 [Usuarios_eft_fullstack_III](https://github.com/NBello26/Usuarios_eft_fullstack_III)
* 📍 [Geolocalizacion_eft_fullstack_III](https://github.com/NBello26/Geolocalizacion_eft_fullstack_III)
* 🔔 [Notificaciones_eft_fullstack_III](https://github.com/NBello26/Notificaciones_eft_fullstack_III)
* 🧩 [Coincidencias_eft_fullstack_III](https://github.com/NBello26/Coincidencias_eft_fullstack_III)

---
*Desarrollado como parte del proyecto final de integración de arquitectura DevOps.*
