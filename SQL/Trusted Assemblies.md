![SQL | Database](https://img.shields.io/badge/SQL-Database-008140?&labelColor=D72638)
![Info | Code Example](https://img.shields.io/badge/Info-Code%20Example-FF8C00?&labelColor=085eb5)

## Pasos
1. Obtener el hash del archivo de la publicación es decir `bd.publish` que se encuentra en carpeta debug posterior a compilar el proyecto
2. Convertir dicho hash en un `varbinary(64)` empleando el algoritmo hash `SHA2_512`
3. Posterior ejecutar el procedimiento `sys.sp_add_trusted_assembly` junto al hash generado anteriormente y una descripción para identificar el DLL de confianza
4. Esto agrega el DLL en confianza para todo el servidor por lo que no se configura individualmente por base de datos
    
```sql
declare @assembly varbinary(max) = 0x
declare @hash varbinary(64) = HASHBYTES('SHA2_512', @assembly);

exec sys.sp_add_trusted_assembly @hash, N'Assembly Name';
```

## Otras funciones de interés sobre los DLL

- Saber cuáles DLL se han marcado como confiables se han creado
    
    ```sql
    select * from sys.trusted_assemblies
    ```
    
- Eliminar un DLL de confianza empleando su hash `varbinary(64)`
    
    ```sql
    DECLARE @hash varbinary(64);
    SET @hash = 0x;
    exec sp_drop_trusted_assembly @hash
    ```