![Info](https://img.shields.io/badge/Info-0031e7) ![Info](https://img.shields.io/badge/Code-027c4d) ![MicrosoftSQLServer](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white&style=flat) 

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