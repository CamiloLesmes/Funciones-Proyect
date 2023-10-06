# Uso_cursor_base_de_datos
```sql
DECLARE @ProductoID int, @Nombre nvarchar(50), @Precio decimal(10,2)
 
-- Crear el cursor
DECLARE RegistroProductos CURSOR FOR
SELECT ProductoID, Nombre, Precio
FROM Productos
WHERE Estado = 'No Registrado' -- Suponiendo que hay un campo 'Estado' que indica si el producto ya ha sido registrado o no.

-- Abrir el cursor
OPEN RegistroProductos

-- Recuperar el primer registro
FETCH NEXT FROM RegistroProductos INTO @ProductoID, @Nombre, @Precio

-- Recorrer todos los registros
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Aquí se realizan las operaciones como por ejemplo, actualizar el estado o registrar en otro historial.
    UPDATE Productos
    SET Estado = 'Registrado'
    WHERE ProductoID = @ProductoID
    
    -- Registrar en un log o historial (suponiendo que existe una tabla llamada 'HistorialRegistro')
    INSERT INTO HistorialRegistro (ProductoID, FechaRegistro, Precio)
    VALUES (@ProductoID, GETDATE(), @Precio)

    -- Recuperar el siguiente registro
    FETCH NEXT FROM RegistroProductos INTO @ProductoID, @Nombre, @Precio
END

-- Cerrar el cursor
CLOSE RegistroProductos
DEALLOCATE RegistroProductos
```

