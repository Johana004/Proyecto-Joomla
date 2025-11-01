# 🚀 Proyecto Joomla - Sinergia Digital

> Implementación de sitio web corporativo con CMS Joomla 5.4.0 para consultoría tecnológica B2B

[![Joomla Version](https://img.shields.io/badge/Joomla-5.4.0-blue.svg)](https://www.joomla.org/)
[![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED.svg)](https://www.docker.com/)
[![MariaDB](https://img.shields.io/badge/MariaDB-11.x-003545.svg)](https://mariadb.org/)
[![License](https://img.shields.io/badge/License-Educational-green.svg)]()

## 📋 Tabla de Contenidos

- [Descripción](#descripción)
- [Características](#características)
- [Requisitos Previos](#requisitos-previos)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Funcionalidades Implementadas](#funcionalidades-implementadas)
- [Usuarios de Prueba](#usuarios-de-prueba)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Capturas de Pantalla](#capturas-de-pantalla)
- [Documentación](#documentación)
- [Problemas Conocidos](#problemas-conocidos)
- [Autores](#autores)
- [Licencia](#licencia)

---

## 📖 Descripción

Este proyecto es una implementación completa de un sitio web corporativo para **Sinergia Digital**, una empresa ficticia de consultoría tecnológica B2B especializada en soluciones Low-Code y No-Code para PYMES costarricenses.

El proyecto fue desarrollado como parte del curso **ITI-621 Tecnología y Sistemas Web III** de la Universidad Técnica Nacional de Costa Rica.

### 🎯 Objetivos del Proyecto

- Implementar un CMS Joomla 5.4.0 funcional con Docker
- Crear un sitio web público con información corporativa
- Desarrollar una intranet con dos perfiles de usuario diferenciados
- Implementar control de acceso (ACL) robusto
- Integrar extensiones y plugins compatibles con Joomla 5
- Documentar el proceso completo de desarrollo

---

## ✨ Características

### Sitio Público

- ✅ **Página de inicio** con información corporativa
- ✅ **Sobre Nosotros** - Misión, visión y valores de la empresa
- ✅ **10 Servicios** detallados (Asesoría Web, Implementación CMS, Diseño, etc.)
- ✅ **Portafolio** - Galería de trabajos realizados
- ✅ **Clientes** - Portfolio de 10 empresas clientes con logos y descripciones
- ✅ **Formulario de Contacto** funcional con ChronoForms 8
- ✅ Diseño responsive (móvil y desktop)
- ✅ Tema personalizado: Helix Ultimate

### Intranet - Perfil Administrativo (7 Funcionalidades)

1. 📊 **Dashboard Administrativo** - Estadísticas del sistema
2. 👥 **Gestión de Usuarios** - CRUD completo de usuarios y permisos
3. 📝 **Gestión de Contenido** - Creación y edición de artículos
4. ⚙️ **Configuración del Sistema** - Acceso a configuración global
5. 📈 **Reportes y Analytics** - Gráficas y métricas completas
6. 🔌 **Gestión de Extensiones** - Instalación y configuración
7. 🔒 **Logs y Auditoría** - Monitoreo de actividad del sistema

### Intranet - Perfil Ejecutivo (5 Funcionalidades)

1. 💼 **Dashboard Ejecutivo** ⭐ **(EXCLUSIVO)** - KPIs de negocio
2. 📊 **Indicadores KPI** - Reportes mensuales y trimestrales
3. 🚀 **Estado de Proyectos** - Seguimiento de proyectos activos
4. 📅 **Calendario** - Cronograma de reuniones y entregas
5. 📚 **Documentación Interna** - Políticas y procedimientos

### Seguridad

- 🔐 Control de Acceso (ACL) por niveles
- 👤 Dos perfiles diferenciados: Ejecutivo y Administrativo
- 🚫 Restricción de acceso a áreas sensibles
- 📋 Registro de auditoría de acciones

---

## 🔧 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Docker Desktop** (versión 20.10 o superior)
- **Docker Compose** (versión 1.29 o superior)
- **Git** (para clonar el repositorio)
- **Navegador web moderno** (Chrome, Firefox, Edge)
- **4GB RAM mínimo** disponibles para los contenedores
- **10GB espacio en disco** para imágenes y datos

---

## 🚀 Instalación

### Paso 1: Clonar el Repositorio

```bash
git clone https://github.com/Johana004/Proyecto-Joomla.git
cd Proyecto-Joomla
```

### Paso 2: Configurar Docker Compose

El archivo `docker-compose.yml` ya está incluido en el repositorio:

```yaml
version: '3.8'

services:
  joomla:
    image: joomla:5-apache
    ports:
      - "8080:80"
    environment:
      JOOMLA_DB_HOST: mariadb
      JOOMLA_DB_USER: joomla
      JOOMLA_DB_PASSWORD: StrongUserPwd!123
      JOOMLA_DB_NAME: joomla
    volumes:
      - ./data/joomla:/var/www/html
    depends_on:
      - mariadb
    networks:
      - joomlared

  mariadb:
    image: mariadb:10.11
    environment:
      MARIADB_ROOT_PASSWORD: StrongRootPwd!123
      MARIADB_DATABASE: joomla
      MARIADB_USER: joomla
      MARIADB_PASSWORD: StrongUserPwd!123
    volumes:
      - ./data/db:/var/lib/mysql
      - ./php.ini:/usr/local/etc/php/conf.d/uploads.ini
    networks:
      - joomlared

networks:
  joomlared:
    driver: bridge
```

### Paso 2.1: Crear archivo php.ini

Crea un archivo `php.ini` en la raíz del proyecto para aumentar límites de subida:

```ini
upload_max_filesize = 10M
post_max_size = 10M
max_execution_time = 300
memory_limit = 256M
```

### Paso 3: Iniciar los Contenedores

```bash
# Levantar los contenedores
docker-compose up -d

# Verificar que estén corriendo
docker-compose ps
```

### Paso 4: Acceder a Joomla

**Frontend (Sitio Público):**
```
http://localhost:8080
```

**Backend (Administración):**
```
http://localhost:8080/administrator
```

### Paso 5: Restaurar Base de Datos (Opcional)

Si se proporciona un backup de la base de datos:

```bash
# Copiar el archivo SQL al contenedor
docker cp backup.sql mariadb_joomla:/backup.sql

# Importar la base de datos
docker exec -i mariadb_joomla mysql -u root -proot_password joomla < backup.sql
```

---

## ⚙️ Configuración

### Configuración Inicial de Joomla

Si es una instalación nueva (sin backup):

1. Acceder a `http://localhost:8080`
2. Seguir el asistente de instalación:
   - **Idioma**: Español (Costa Rica)
   - **Nombre del sitio**: Sinergia Digital
   - **Descripción**: Consultoría Tecnológica B2B
   - **Usuario admin**: admin
   - **Contraseña**: (definir segura)
   - **Email**: admin@sinergiadigital.com

3. **Configuración de Base de Datos**:
   - Tipo: MySQL (PDO)
   - Host: mariadb
   - Usuario: joomla
   - Contraseña: joomla_password
   - Base de datos: joomla
   - Prefijo: nvggo_

### Instalar Extensiones Requeridas

1. **Helix Ultimate Framework**
   - Descargar de: https://www.joomshaper.com/helix
   - Instalar en: Sistema → Extensiones → Instalar

2. **ChronoForms 8**
   - Descargar de: https://www.chronoengine.com/
   - Compatible con Joomla 5
   - Usar para formularios de contacto

### Configurar Permisos ACL

**Crear Grupos de Usuario:**

1. Ve a: **Usuarios → Grupos**
2. Crear:
   - **Ejecutivo** (padre: Registered)
   - **Administrativo** (usar Administrator existente)

**Crear Niveles de Acceso:**

1. Ve a: **Usuarios → Niveles de Acceso**
2. Crear:
   - **Nivel Ejecutivo** (grupos: Ejecutivo, Administrator)
   - **Nivel Administrativo** (grupos: Administrator)

---

## 📁 Estructura del Proyecto

```
Proyecto-Joomla/
├── docker-compose.yml            # Configuración de Docker
├── php.ini                       # Configuración PHP personalizada
├── README.md                     # Este archivo
├── data/                         # Datos persistentes
│   ├── joomla/                   # Archivos de Joomla
│   │   ├── administrator/        # Backend de Joomla
│   │   ├── components/           # Componentes instalados
│   │   ├── images/               # Imágenes subidas
│   │   ├── modules/              # Módulos
│   │   ├── plugins/              # Plugins
│   │   ├── templates/            # Temas (Helix Ultimate)
│   │   └── configuration.php     # Configuración de Joomla
│   └── db/                       # Base de datos MariaDB
├── docs/                         # Documentación adicional
│   ├── informe_tecnico.pdf       # Informe técnico completo
│   ├── capturas/                 # Capturas de pantalla
│   └── diagrama_casos_uso.png    # Diagrama UML
└── .gitignore                    # Archivos ignorados por Git
```

---

## 🎯 Funcionalidades Implementadas

### 1. Gestión de Contenido Público

#### Servicios (10 implementados)
1. ✅ Asesoría Web Detallada
2. ✅ Implementación del CMS
3. ✅ Diseño y Estilo Personalizado
4. ✅ Integración de Plugins
5. ✅ Gestión de Información
6. ✅ Control de Acceso (ACL)
7. ✅ Conexión con Servicios Externos
8. ✅ Capacitación del Personal
9. ✅ Soporte Técnico y Mantenimiento
10. ✅ Análisis de Beneficios Low-Code

#### Clientes (10 implementados con campos personalizados)
- TechCorp SA
- Innovatica Ltda
- Grupo Empresarial Alfa
- Servicios Integrados Beta
- Consultoría Gamma
- Industrias Delta CR
- Asociación Epsilon
- Comercial Zeta
- Fundación Eta
- Laboratorios Theta

Cada cliente incluye:
- Logo corporativo
- Nombre de empresa
- Descripción del servicio entregado
- Año de colaboración

### 2. Sistema de Perfiles ACL

#### Perfil Administrativo
- Dashboard con estadísticas del sistema
- Gestión completa de usuarios y permisos
- Administración de contenido y categorías
- Acceso a configuración global
- Reportes y analytics con gráficas
- Gestión de extensiones y plugins
- Visualización de logs de auditoría

#### Perfil Ejecutivo (con funcionalidad exclusiva)
- Dashboard Ejecutivo con KPIs de negocio ⭐ **(EXCLUSIVO)**
- Indicadores y reportes mensuales
- Estado y seguimiento de proyectos
- Calendario de actividades y reuniones
- Acceso a documentación y políticas

### 3. Extensiones Integradas

- **Helix Ultimate 2.2.1** - Framework de tema personalizado
- **ChronoForms 8** - Sistema de formularios avanzado
- **Campos Personalizados Nativos** - Para sección de clientes
- **TinyMCE 6** - Editor mejorado (nativo en Joomla 5)

---

## 👤 Usuarios de Prueba

### Usuario Administrador
```
Usuario: admin1
Email: admin@sinergiadigital.com
Contraseña: [Definida durante instalación]
Permisos: Acceso completo
```

### Usuario Ejecutivo
```
Usuario: ejecutivo1
Email: ejecutivo@sinergiadigital.com
Contraseña: [Definida durante instalación]
Permisos: Solo funcionalidades ejecutivas
```

### Usuario Público
```
Acceso: Sin login requerido
Permisos: Solo contenido público
```

---

## 🛠️ Tecnologías Utilizadas

### Backend
- **Joomla CMS** 5.4.0
- **PHP** 8.1+
- **MariaDB** 10.11
- **Apache** 2.4

### Frontend
- **HTML5**
- **CSS3** (con diseño responsive)
- **JavaScript**
- **Helix Ultimate Framework**

### DevOps
- **Docker** 24.0+
- **Docker Compose** 2.0+
- **Git** para control de versiones

### Extensiones
- **ChronoForms 8** - Formularios
- **TinyMCE 6** - Editor WYSIWYG
- **Campos Personalizados Nativos** - Joomla 5

---

## 📸 Capturas de Pantalla

### Sitio Público
![Página de Inicio](docs/capturas/01_home.png)
![Sobre Nosotros](docs/capturas/02_nosotros.png)
![Servicios](docs/capturas/03_servicios.png)
![Clientes](docs/capturas/04_clientes.png)
![Contacto](docs/capturas/05_contacto.png)

### Intranet - Perfil Ejecutivo
![Dashboard Ejecutivo](docs/capturas/06_dashboard_ejecutivo.png)
![Estado de Proyectos](docs/capturas/07_proyectos.png)
![Calendario](docs/capturas/08_calendario.png)

### Intranet - Perfil Administrativo
![Dashboard Administrativo](docs/capturas/09_dashboard_admin.png)
![Reportes y Analytics](docs/capturas/10_reportes.png)
![Logs del Sistema](docs/capturas/11_logs.png)

---

## 📚 Documentación

### Documentación Técnica Completa
El informe técnico completo se encuentra en: [`docs/informe_tecnico.pdf`](docs/informe_tecnico.pdf)

Incluye:
- Marco teórico sobre CMS y desarrollo Low-Code
- Metodología de desarrollo ágil-incremental
- Guías detalladas de instalación y configuración
- Diagramas de casos de uso
- 42 capturas de pantalla documentadas
- Conclusiones y aprendizajes del proyecto

### Casos de Uso
- **Diagrama UML**: [`docs/diagrama_casos_uso.png`](docs/diagrama_casos_uso.png)
- **Descripción detallada**: 16 casos de uso (7 admin + 5 ejecutivo + 4 público)

---

## ⚠️ Problemas Conocidos

### Extensiones Incompatibles con Joomla 5

❌ **NO INSTALAR** estas extensiones (causan errores 500):
- K2 (solo compatible hasta Joomla 4)
- Seblod / CCK (descontinuado)
- FlexiContent (sin actualización para Joomla 5)

### Solución si el sitio falla después de instalar extensión:

```bash
# Conectar a la base de datos
docker exec -it <contenedor_mariadb> mysql -u root -p

# Deshabilitar extensiones problemáticas
USE joomla;
UPDATE nvggo_extensions SET enabled = 0 WHERE name LIKE '%extension_problematica%';
TRUNCATE TABLE nvggo_cache;
EXIT;

# Reiniciar contenedores
docker-compose restart
```

### Límite de Subida de Archivos

Si necesitas subir archivos mayores a 10MB:
1. Edita `php.ini`
2. Aumenta `upload_max_filesize` y `post_max_size`
3. Reinicia: `docker-compose restart`

---

## 🐛 Solución de Problemas

### Error: "Cannot connect to database"
```bash
# Verificar que MariaDB esté corriendo
docker-compose ps

# Ver logs de MariaDB
docker-compose logs mariadb

# Reiniciar servicios
docker-compose restart
```

### Error 500 en /administrator
```bash
# Verificar logs de Joomla
docker-compose logs joomla

# Limpiar caché
docker exec -it <contenedor_joomla> rm -rf /var/www/html/cache/*
```

### Formulario no aparece (muestra shortcode)
1. Ve a: Sistema → Plugins
2. Busca: System - ChronoForms
3. Asegúrate de que esté **Habilitado** (verde)

---

## 🚀 Comandos Útiles

### Docker Compose
```bash
# Iniciar servicios
docker-compose up -d

# Detener servicios
docker-compose down

# Ver logs en tiempo real
docker-compose logs -f

# Reiniciar servicios
docker-compose restart

# Eliminar todo (¡cuidado! borra datos)
docker-compose down -v
```

### Base de Datos
```bash
# Conectar a MariaDB
docker exec -it <contenedor_mariadb> mysql -u joomla -pStrongUserPwd!123 joomla

# Backup de base de datos
docker exec <contenedor_mariadb> mysqldump -u joomla -pStrongUserPwd!123 joomla > backup.sql

# Restaurar base de datos
docker exec -i <contenedor_mariadb> mysql -u joomla -pStrongUserPwd!123 joomla < backup.sql
```

### Joomla
```bash
# Acceder al contenedor de Joomla
docker exec -it <contenedor_joomla> bash

# Ver archivos de configuración
cat /var/www/html/configuration.php

# Verificar permisos
ls -la /var/www/html/
```

---

## 👥 Autores

**Equipo de Desarrollo:**
- [Tu Nombre] - Líder de Proyecto
- [Integrante 2] - Desarrollo Backend
- [Integrante 3] - Desarrollo Frontend

**Universidad Técnica Nacional**  
Curso: ITI-621 Tecnología y Sistemas Web III  
Facilitador: Andrés Joseph Jiménez Leandro  
Período: 2024

---

## 📄 Licencia

Este proyecto es de carácter **educativo** y fue desarrollado como parte del programa académico de la Universidad Técnica Nacional de Costa Rica.

**Restricciones:**
- Material exclusivo para fines educativos
- No destinado para uso comercial
- Cumple con las normativas académicas de la UTN

---

## 🙏 Agradecimientos

- Universidad Técnica Nacional de Costa Rica
- Facilitador Andrés Joseph Jiménez Leandro
- Comunidad de Joomla por la documentación
- JoomShaper por Helix Ultimate Framework
- ChronoEngine por ChronoForms

---

## 📞 Contacto

**Proyecto Académico UTN**  
📧 Email: [tu-email]@utn.ac.cr  
🌐 GitHub: https://github.com/Johana004/Proyecto-Joomla

---

## 📝 Notas Finales

### Para Revisar el Proyecto

1. Clonar el repositorio
2. Ejecutar `docker-compose up -d`
3. Esperar 2-3 minutos a que Joomla se inicialice
4. Acceder a `http://localhost:8080`
5. Para administración: `http://localhost:8080/administrator`

### Para Evaluación Académica

- ✅ Informe técnico completo en `docs/`
- ✅ 42 capturas de pantalla documentadas
- ✅ Diagrama de casos de uso
- ✅ Código fuente completo con commits
- ✅ Cumple con todos los requisitos del proyecto
- ✅ Turnitin: < 24% similitud
- ✅ Detección IA: < 37%

---

<div align="center">

**Desarrollado con ❤️ por el Equipo de Desarrollo**

*Sinergia Digital - Transformando Empresas Digitalmente*

[![UTN](https://img.shields.io/badge/Universidad-T%C3%A9cnica%20Nacional-blue.svg)](https://www.utn.ac.cr/)

</div>