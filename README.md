# Laboratorio: Código Seguro y Modelo OWASP

## 1. Datos generales

- **Nombre completo:** Jonathan David Reyes Hernandez
- **Carné:** rh
- **Correo de contacto:** hedilia77@gmail.com
- **Asignatura:** Lenguaje de programación en aspectos de seguridad
- **Fecha:** 08/11/2026
- **GitHub:** [Jonathanreyes-pixel](https://github.com/Jonathanreyes-pixel)

## 2. Descripción

Este laboratorio consiste en desarrollar una aplicación web utilizando Python, implementando controles básicos de seguridad basados en buenas prácticas y el modelo OWASP. Se aplican mecanismos de validación de entradas, consultas parametrizadas, protección de contraseñas, registro de eventos de autenticación y detección de incidentes, incluyendo el cálculo de las métricas MTTD y MTTR.

## 3. Estructura del proyecto

```text
laboratorio-las/
|-- app.py
|-- crearDB.py
|-- detector.py
|-- requirements.txt
|-- README.md
|-- resultados_pruebas.md
|-- usuarios.db
|-- eventos.log
|-- respuesta.log
|-- templates/
    |-- login.html
    |-- registro.html
```

El entorno local `venv/` no se incluye en GitHub; se crea siguiendo las instrucciones. La base usuarios.db permanece local porque contiene credenciales; se crea con crearDB.py.

## 4. Ejecución del proyecto

Requisito: Python 3.14. Los comandos siguientes se ejecutan en CMD de Windows.

1. Clonar el repositorio e ingresar en su carpeta:

```bat
git clone https://github.com/Jonathanreyes-pixel/laboratorio-las.git
cd laboratorio-las
```
2. Crear y activar el entorno virtual:

```bat
python -m venv venv
venv\Scripts\activate.bat
```

3. Instalar las dependencias e inicializar SQLite:

```bat
python -m pip install -r requirements.txt
python crearDB.py
```

4. Ejecutar la aplicación:

```bat
python app.py
```

Registro: http://127.0.0.1:5000/registro

Inicio de sesión: http://127.0.0.1:5000/login

5. En otra terminal CMD, activar el entorno y ejecutar el detector:

```bat
venv\Scripts\activate.bat
python detector.py
```

Se deben generar al menos tres accesos fallidos para un mismo usuario. El detector lee `eventos.log` y registra una respuesta simulada en `respuesta.log`.

## 5. Controles implementados

- Usuario de 4 a 20 caracteres: letras, números y guion bajo.
- Validación del formato del correo y contraseña de al menos 8 caracteres.
- Normalización de usuario y correo, conservando los espacios de la contraseña.
- Consultas SQL parametrizadas y comprobación de usuarios o correos duplicados.
- Hash de contraseñas con Werkzeug y verificación mediante `check_password_hash`.
- Registro de accesos correctos y fallidos con fecha, tipo y usuario.
- Alerta por tres o más fallos de un usuario y registro de bloqueo simulado.

Los usuarios creados antes del cambio a hash conservan el formato anterior y no son compatibles con el inicio de sesión actual. El bloqueo del laboratorio es simulado; no restringe accesos en Flask.

## 6. Resultados de las pruebas

| Prueba | MTTD (segundos) | MTTR (segundos) |
|---|---:|---:|
| 1 | 2 | 2 |
| 2 | 4 | 3 |
| 3 | 6 | 4 |
| **Promedio** | **4** | **3** |

- MTTD: fecha del tercer fallo menos fecha del primer fallo.
- MTTR: fecha de respuesta simulada menos fecha del tercer fallo.
- Promedio: suma de los valores dividida entre tres pruebas.

Las pausas entre fallos fueron de 1, 2 y 3 segundos; las esperas antes de ejecutar el detector fueron de 2, 3 y 4 segundos. Se tomó la primera respuesta de cada escenario. Cada ejecución del detector vuelve a procesar los eventos históricos y agrega respuestas nuevas.

El detalle está en [resultados_pruebas.md](resultados_pruebas.md).

## 7. Entrega

El repositorio debe ser público y contener el código completo. Antes de entregar, comprobar que todos los cambios están sincronizados y que el enlace pueda abrirse sin iniciar sesión. Pegar únicamente el enlace del repositorio en el Aula Virtual, sin archivos comprimidos.
