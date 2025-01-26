# library-node
El objetivo de esta prueba técnica es evaluar habilidades en el desarrollo de una API RESTful utilizando Node.js, Express y Sequelize como ORM. Deberás implementar una serie de endpoints que permitan gestionar y consultar información relacionada con una biblioteca, incluyendo libros, categorías, sucursales, préstamos y promociones.
![image](https://github.com/user-attachments/assets/5ebb38df-ff94-4733-9938-a0a7c52e6f9f)

### **Prueba Técnica Node.js con Express y Sequelize**

#### **Preparación del entorno**

1. **Crear un repositorio en GitHub** con el nombre `TuNombre-prueba-node`. Por ejemplo, `Juan-Perez-prueba-node`.
2. **Crear un proyecto Node.js** con Express y Sequelize como ORM.
3. **Montar la base de datos** "Library" (se adjunta el modelo relacional al final).

---

#### **Endpoints a implementar**

1. **Listar libros con su disponibilidad en cada sucursal**  
   - Crear un endpoint que permita listar los libros con sus datos básicos y la información de disponibilidad en cada sucursal.
   - La disponibilidad se obtiene de la tabla `LIBROS_STOCK`, donde se relaciona el libro con la sucursal y la cantidad disponible.

2. **Listar los 5 libros más prestados**  
   - Crear un endpoint que liste los 5 libros más prestados en orden descendente por la cantidad de veces que han sido prestados.
   - Para obtener la cantidad de veces que un libro ha sido prestado, se debe consultar la tabla `PRESTAMOS`.

3. **Listar categorías con al menos un libro**  
   - Crear un endpoint que liste las categorías que tengan al menos un libro, ordenadas de forma descendente por la cantidad de libros en cada categoría.

4. **Listar promociones activas según un día de la semana**  
   - Crear un endpoint que permita listar las promociones activas según un día de la semana.
   - En la tabla `PROMOCIONES`, la columna `DIAS_SEMANA` contiene un array con las posiciones de los días de la semana (0 = No aplica, 1 = Aplica), empezando por el lunes.
   - También se deben obtener las sucursales donde la promoción está disponible, según las fechas de inicio y fin en la tabla `SUCURSALES_PROMOCIONES`.

---

#### **Entrega Final**

- **Subir el proyecto a un repositorio GitHub público**.
- **Incluir un archivo README.md** con:
  - Configuración del entorno.
  - Instrucciones para ejecutar la API.
- **Opcional**: Incluir scripts SQL si es necesario.

---

#### **Modelo Relacional de la Base de Datos "Library"**

1. **Tabla `LIBROS`**
   - `id`: INT (PK)
   - `titulo`: VARCHAR(100)
   - `autor`: VARCHAR(100)
   - `descripcion`: TEXT
   - `foto`: VARCHAR(120)

2. **Tabla `CATEGORIAS`**
   - `id`: INT (PK)
   - `nombre`: VARCHAR(50)

3. **Tabla `LIBROS_CATEGORIAS`**
   - `id`: INT (PK)
   - `id_libro`: INT (FK a LIBROS)
   - `id_categoria`: INT (FK a CATEGORIAS)

4. **Tabla `SUCURSALES`**
   - `id`: INT (PK)
   - `nombre`: VARCHAR(100)
   - `direccion`: VARCHAR(200)

5. **Tabla `LIBROS_STOCK`**
   - `id`: INT (PK)
   - `id_libro`: INT (FK a LIBROS)
   - `id_sucursal`: INT (FK a SUCURSALES)
   - `cantidad`: INT

6. **Tabla `PRESTAMOS`**
   - `id`: INT (PK)
   - `id_libro`: INT (FK a LIBROS)
   - `id_usuario`: INT (FK a USUARIOS)
   - `fecha_prestamo`: DATE
   - `fecha_devolucion`: DATE

7. **Tabla `PROMOCIONES`**
   - `id`: INT (PK)
   - `nombre`: VARCHAR(100)
   - `descripcion`: TEXT
   - `dias_semana`: VARCHAR(21) (Array de 7 posiciones, 0 o 1)
   - `fecha_inicio`: DATE
   - `fecha_fin`: DATE

8. **Tabla `SUCURSALES_PROMOCIONES`**
   - `id`: INT (PK)
   - `id_promocion`: INT (FK a PROMOCIONES)
   - `id_sucursal`: INT (FK a SUCURSALES)

---

### **Ejemplo de Endpoint**

Aquí tienes un ejemplo de cómo podrías estructurar uno de los endpoints:

```javascript
// Endpoint para listar libros con su disponibilidad en cada sucursal
app.get('/libros-disponibilidad', async (req, res) => {
    try {
        const libros = await Libro.findAll({
            include: [
                {
                    model: Sucursal,
                    through: { attributes: ['cantidad'] } // Incluye la cantidad disponible en cada sucursal
                }
            ]
        });
        res.json(libros);
    } catch (error) {
        res.status(500).json({ error: 'Error al obtener los libros' });
    }
});
```

---

### **Consejos para la implementación**

1. **Configura Sequelize** correctamente para conectarte a la base de datos.
2. **Define los modelos** de Sequelize según el modelo relacional proporcionado.
3. **Usa migraciones** para crear las tablas en la base de datos.
4. **Prueba tus endpoints** con herramientas como Insomnia o Thunder Client.
