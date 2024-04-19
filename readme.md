# Consultas sobre una tabla:
### 1. Lista el primer apellido de todos los empleados.
```SQL:
SELECT apellido1 FROM empleado;
```
### 2. Lista el primer apellido de los empleados eliminando los apellidos que estén repetidos.
```SQL:
SELECT DISTINCT apellido1 FROM empleado;
```
### 3. Lista todas las columnas de la tabla empleado.
```SQL:
SHOW COLUMNS FROM empleado;
```
### 4. Lista el nombre y los apellidos de todos los empleados.
```SQL:
SELECT nombre, apellido1, apellido2 FROM empleado;
```
### 5. Lista el identificador de los departamentos de los empleados que aparecen en la tabla empleado.
```SQL:
SELECT codigo_departamento FROM empleado;
```
### 6. Lista el identificador de los departamentos de los empleados que aparecen en la tabla empleado, eliminando los identificadores que aparecen repetidos.
```SQL:
SELECT DISTINCT codigo_departamento FROM empleado;
```
### 7. Lista el nombre y apellidos de los empleados en una única columna.
```SQL:
SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS nombre_completo FROM empleado;
```
### 8. Lista el nombre y apellidos de los empleados en una única columna, convirtiendo todos los caracteres en mayúscula.
```SQL:
SELECT UPPER(CONCAT(nombre, ' ', apellido1, ' ', COALESCE(apellido2, ''))) AS nombre_completo_mayuscula FROM empleado;
```
### 9. Lista el nombre y apellidos de los empleados en una única columna, convirtiendo todos los caracteres en minúscula.
```SQL:
SELECT LOWER(CONCAT(nombre, ' ', apellido1, ' ', COALESCE(apellido2, ''))) AS nombre_completo_minuscula FROM empleado;
```
### 10. Lista el identificador de los empleados junto al nif, pero el nif deberá aparecer en dos columnas, una mostrará únicamente los dígitos del nif y la otra la letra.
```SQL:
SELECT codigo, 
       LEFT(nif, LENGTH(nif) - 1) AS nif_digitos, 
       RIGHT(nif, 1) AS nif_letra 
FROM empleado;
```
### 11. Lista el nombre de cada departamento y el valor del presupuesto actual del que dispone. Para calcular este dato tendrá que restar al valor del presupuesto inicial (columna presupuesto) los gastos que se han generado (columna gastos). Tenga en cuenta que en algunos casos pueden existir valores negativos. Utilice un alias apropiado para la nueva columna columna que está calculando.
```SQL:
SELECT nombre, (presupuesto - COALESCE(gastos, 0)) AS presupuesto_actual
FROM departamento
LEFT JOIN (
    SELECT codigo_departamento, SUM(presupuesto) AS gastos
    FROM empleado
    GROUP BY codigo_departamento
) AS gastos_departamento ON departamento.codigo = gastos_departamento.codigo_departamento;

```
### 12. Lista el nombre de los departamentos y el valor del presupuesto actual ordenado de forma ascendente.
```SQL:
SELECT nombre, (presupuesto - COALESCE(gastos, 0)) AS presupuesto_actual
FROM departamento
LEFT JOIN (
    SELECT codigo_departamento, SUM(presupuesto) AS gastos
    FROM empleado
    GROUP BY codigo_departamento
) AS gastos_departamento ON departamento.codigo = gastos_departamento.codigo_departamento
ORDER BY presupuesto_actual ASC;

```
### 13. Lista el nombre de todos los departamentos ordenados de forma ascendente.
```SQL:
SELECT nombre FROM departamento ORDER BY nombre ASC;
```
### 14. Lista el nombre de todos los departamentos ordenados de forma descendente.
```SQL:
SELECT nombre FROM departamento ORDER BY nombre DESC;
```
### 15. Lista los apellidos y el nombre de todos los empleados, ordenados de forma alfabética tendiendo en cuenta en primer lugar sus apellidos y luego su nombre.
```SQL:
SELECT apellido1, apellido2, nombre
FROM empleado
ORDER BY apellido1 ASC, apellido2 ASC, nombre ASC;
```
### 16. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos que tienen mayor presupuesto.
```SQL:
SELECT nombre, presupuesto
FROM departamento
ORDER BY presupuesto DESC
LIMIT 3;
```
### 17. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos que tienen menor presupuesto.
```SQL:
SELECT nombre, presupuesto
FROM departamento
ORDER BY presupuesto ASC
LIMIT 3;
```
### 18. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que tienen mayor gasto.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS gasto_total
FROM empleado AS e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
ORDER BY gasto_total DESC
LIMIT 2;
```
### 19. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que tienen menor gasto.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS gasto_total
FROM empleado AS e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
ORDER BY gasto_total ASC
LIMIT 2;
```
### 20. Devuelve una lista con 5 filas a partir de la tercera fila de la tabla empleado. La tercera fila se debe incluir en la respuesta. La respuesta debe incluir todas las columnas de la tabla empleado.
```SQL:
SELECT *
FROM empleado as e
LIMIT 2, 5;

```
### 21. Devuelve una lista con el nombre de los departamentos y el presupuesto, de aquellos que tienen un presupuesto mayor o igual a 150000 euros.
```SQL:
SELECT nombre, presupuesto
FROM departamento
WHERE presupuesto >= 150000;
```
### 22. Devuelve una lista con el nombre de los departamentos y el gasto, de aquellos que tienen menos de 5000 euros de gastos.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS gasto_total
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
HAVING gasto_total < 5000;
```
### 23. Devuelve una lista con el nombre de los departamentos y el presupuesto, de aquellos que tienen un presupuesto entre 100000 y 200000 euros. Sin utilizar el operador BETWEEN.
```SQL:
SELECT nombre, presupuesto
FROM departamento
WHERE presupuesto >= 100000 AND presupuesto <= 200000;
```
### 24. Devuelve una lista con el nombre de los departamentos que no tienen un presupuesto entre 100000 y 200000 euros. Sin utilizar el operador BETWEEN.
```SQL:
SELECT nombre
FROM departamento
WHERE presupuesto < 100000 OR presupuesto > 200000;
```
### 25. Devuelve una lista con el nombre de los departamentos que tienen un presupuesto entre 100000 y 200000 euros. Utilizando el operador BETWEEN.
```SQL:
SELECT nombre
FROM departamento
WHERE presupuesto BETWEEN 100000 AND 200000;
```
### 26. Devuelve una lista con el nombre de los departamentos que no tienen un presupuesto entre 100000 y 200000 euros. Utilizando el operador BETWEEN.
```SQL:
SELECT nombre
FROM departamento
WHERE presupuesto NOT BETWEEN 100000 AND 200000;
```
### 27. Devuelve una lista con el nombre de los departamentos, gastos y presupuesto, de aquellos departamentos donde los gastos sean mayores que el presupuesto del que disponen.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS presupuesto, SUM(e.gastos) AS gastos
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
HAVING gastos > presupuesto;

```
### 28. Devuelve una lista con el nombre de los departamentos, gastos y presupuesto, de aquellos departamentos donde los gastos sean menores que el presupuesto del que disponen.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS presupuesto, SUM(e.gastos) AS gastos
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
HAVING gastos < presupuesto;
```
### 29. Devuelve una lista con el nombre de los departamentos, gastos y presupuesto, de aquellos departamentos donde los gastos sean iguales al presupuesto del que disponen.
```SQL:
SELECT d.nombre, SUM(e.presupuesto) AS presupuesto, SUM(e.gastos) AS gastos
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
GROUP BY e.codigo_departamento
HAVING gastos = presupuesto;
```
### 30. Lista todos los datos de los empleados cuyo segundo apellido sea NULL.
```SQL:
SELECT *
FROM empleado
WHERE apellido2 IS NULL;
```
### 31. Lista todos los datos de los empleados cuyo segundo apellido no sea NULL.
```SQL:
SELECT *
FROM empleado
WHERE apellido2 IS NOT NULL;
```
### 32. Lista todos los datos de los empleados cuyo segundo apellido sea López.
```SQL:
SELECT *
FROM empleado
WHERE apellido2 = 'López';
```
### 33. Lista todos los datos de los empleados cuyo segundo apellido sea Díaz o Moreno. Sin utilizar el operador IN.
```SQL:
SELECT *
FROM empleado
WHERE apellido2 = 'Díaz' OR apellido2 = 'Moreno';
```
### 34. Lista todos los datos de los empleados cuyo segundo apellido sea Díaz o Moreno. Utilizando el operador IN.
```SQL:
SELECT *
FROM empleado
WHERE apellido2 IN ('Díaz', 'Moreno');
```
### 35. Lista los nombres, apellidos y nif de los empleados que trabajan en el departamento 3.
```SQL:
SELECT nombre, apellido1, apellido2, nif
FROM empleado
WHERE codigo_departamento = 3;
```
### 36. Lista los nombres, apellidos y nif de los empleados que trabajan en los departamentos 2, 4 o 5.
```SQL:
SELECT nombre, apellido1, apellido2, nif
FROM empleado
WHERE codigo_departamento IN (2, 4, 5);
```
# Consultas Multitabla (Composiciòn Interna):

### 1. Devuelve un listado con los empleados y los datos de los departamentos donde trabaja cada uno.
```SQL:
SELECT e.nombre, e.apellido1, e.apellido2, e.nif, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo;
```
### 2. Devuelve un listado con los empleados y los datos de los departamentos donde trabaja cada uno. Ordena el resultado, en primer lugar por el nombre del departamento (en orden alfabético) y en segundo lugar por los apellidos y el nombre de los empleados.
```SQL:
SELECT e.nombre, e.apellido1, e.apellido2, e.nif, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
ORDER BY nombre_departamento ASC, e.apellido1 ASC, e.apellido2 ASC, e.nombre ASC;
```
### 3. Devuelve un listado con el identificador y el nombre del departamento, solamente de aquellos departamentos que tienen empleados.
```SQL:
SELECT d.codigo, d.nombre
FROM departamento d
INNER JOIN empleado e ON d.codigo = e.codigo_departamento
GROUP BY d.codigo, d.nombre;
```
### 4. Devuelve un listado con el identificador, el nombre del departamento y el valor del presupuesto actual del que dispone, solamente de aquellos departamentos que tienen empleados. El valor del presupuesto actual lo puede calcular restando al valor del presupuesto inicial (columna presupuesto) el valor de los gastos que ha generado (columna gastos).
```SQL:
SELECT d.codigo AS identificador, d.nombre AS nombre_departamento, 
       (d.presupuesto - COALESCE(SUM(e.gastos), 0)) AS presupuesto_actual
FROM departamento d
INNER JOIN empleado e ON d.codigo = e.codigo_departamento
GROUP BY d.codigo, d.nombre, d.presupuesto;
```
### 5. Devuelve el nombre del departamento donde trabaja el empleado que tiene el nif 38382980M.
```SQL:
SELECT d.nombre AS nombre_departamento
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE e.nif = '38382980M';
```
### 6. Devuelve el nombre del departamento donde trabaja el empleado Pepe Ruiz Santana.
```SQL:
SELECT d.nombre AS nombre_departamento
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE e.nombre = 'Pepe' AND e.apellido1 = 'Ruiz' AND e.apellido2 = 'Santana';
```
### 7. Devuelve un listado con los datos de los empleados que trabajan en el departamento de I+D. Ordena el resultado alfabéticamente.
```SQL:
SELECT *
FROM empleado
WHERE codigo_departamento = (
    SELECT codigo
    FROM departamento
    WHERE nombre = 'I+D'
)
ORDER BY nombre ASC, apellido1 ASC, apellido2 ASC;
```
### 8. Devuelve un listado con los datos de los empleados que trabajan en el departamento de Sistemas, Contabilidad o I+D. Ordena el resultado alfabéticamente.
```SQL:
SELECT *
FROM empleado
WHERE codigo_departamento IN (
    SELECT codigo
    FROM departamento
    WHERE nombre IN ('Sistemas', 'Contabilidad', 'I+D')
)
ORDER BY nombre ASC, apellido1 ASC, apellido2 ASC;
```
### 9. Devuelve una lista con el nombre de los empleados que tienen los departamentos que no tienen un presupuesto entre 100000 y 200000 euros.
```SQL:
SELECT e.nombre
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE d.codigo NOT IN (
    SELECT codigo
    FROM departamento
    WHERE presupuesto BETWEEN 100000 AND 200000
);
```
### 10. Devuelve un listado con el nombre de los departamentos donde existe algún empleado cuyo segundo apellido sea NULL. Tenga en cuenta que no debe mostrar nombres de departamentos que estén repetidos.
```SQL:
SELECT DISTINCT d.nombre
FROM departamento d
JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE e.apellido2 IS NULL;
```

# Consultas multitabla (Composición externa):

### 1. Devuelve un listado con todos los empleados junto con los datos de los departamentos donde trabajan. Este listado también debe incluir los empleados que no tienen ningún departamento asociado.
```SQL:
SELECT e.*, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM empleado e
LEFT JOIN departamento d ON e.codigo_departamento = d.codigo;
```
### 2. Devuelve un listado donde sólo aparezcan aquellos empleados que no tienen ningún departamento asociado.
```SQL:
SELECT e.*
FROM empleado e
LEFT JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE d.codigo IS NULL;
```
### 3. Devuelve un listado donde sólo aparezcan aquellos departamentos que no tienen ningún empleado asociado.
```SQL:
SELECT d.*
FROM departamento d
LEFT JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE e.codigo_departamento IS NULL;
```
### 4. Devuelve un listado con todos los empleados junto con los datos de los departamentos donde trabajan. El listado debe incluir los empleados que no tienen ningún departamento asociado y los departamentos que no tienen ningún empleado asociado. Ordene el listado alfabéticamente por el nombre del departamento.
```SQL:
-- Empleados con departamento asociado
SELECT e.*, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM empleado e
LEFT JOIN departamento d ON e.codigo_departamento = d.codigo

UNION ALL

-- Empleados sin departamento asociado
SELECT e.*, NULL AS nombre_departamento, NULL AS presupuesto_departamento
FROM empleado e
WHERE e.codigo_departamento IS NULL

UNION ALL

-- Departamentos sin empleados asociados
SELECT NULL AS codigo, NULL AS nif, NULL AS nombre, NULL AS apellido1, NULL AS apellido2, d.codigo AS codigo_departamento, d.presupuesto, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM departamento d
LEFT JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE e.codigo_departamento IS NULL

ORDER BY nombre_departamento ASC, nombre ASC, apellido1 ASC, apellido2 ASC;
```
### 5. Devuelve un listado con los empleados que no tienen ningún departamento asociado y los departamentos que no tienen ningún empleado asociado. Ordene el listado alfabéticamente por el nombre del departamento.
```SQL:
-- Empleados sin departamento asociado
SELECT e.*, NULL AS nombre_departamento, NULL AS presupuesto_departamento
FROM empleado e
WHERE e.codigo_departamento IS NULL

UNION ALL

-- Departamentos sin empleados asociados
SELECT NULL AS codigo, NULL AS nif, NULL AS nombre, NULL AS apellido1, NULL AS apellido2, d.codigo AS codigo_departamento, d.presupuesto, d.nombre AS nombre_departamento, d.presupuesto AS presupuesto_departamento
FROM departamento d
LEFT JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE e.codigo_departamento IS NULL

ORDER BY nombre_departamento ASC;
```
### 6. Devuelve el nombre del departamento donde trabaja el empleado Pepe Ruiz Santana.
```SQL:
SELECT d.nombre AS nombre_departamento
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE e.nombre = 'Pepe' AND e.apellido1 = 'Ruiz' AND e.apellido2 = 'Santana';
```
### 7. Devuelve un listado con los datos de los empleados que trabajan en el departamento de I+D. Ordena el resultado alfabéticamente.
```SQL:
SELECT e.*
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE d.nombre = 'I+D'
ORDER BY e.nombre ASC;
```
### 8. Devuelve un listado con los datos de los empleados que trabajan en el departamento de Sistemas, Contabilidad o I+D. Ordena el resultado alfabéticamente.
```SQL:
SELECT e.*
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE d.nombre IN ('Sistemas', 'Contabilidad', 'I+D')
ORDER BY e.nombre ASC;
```
### 9. Devuelve una lista con el nombre de los empleados que tienen los departamentos que no tienen un presupuesto entre 100000 y 200000 euros.
```SQL:
SELECT e.nombre
FROM empleado e
JOIN departamento d ON e.codigo_departamento = d.codigo
WHERE d.presupuesto < 100000 OR d.presupuesto > 200000;
```
### 10. Devuelve un listado con el nombre de los departamentos donde existe algún empleado cuyo segundo apellido sea NULL. Tenga en cuenta que no debe mostrar nombres de departamentos que estén repetidos.
```SQL:
SELECT DISTINCT d.nombre
FROM departamento d
JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE e.apellido2 IS NULL;
```

# Consultas resumen:

### 1. Calcula la suma del presupuesto de todos los departamentos.
```SQL:
SELECT SUM(presupuesto) AS suma_presupuesto
FROM departamento;
```
### 2. Calcula la media del presupuesto de todos los departamentos.
```SQL:
SELECT AVG(presupuesto) AS media_presupuesto
FROM departamento;
```
### 3. Calcula el valor mínimo del presupuesto de todos los departamentos.
```SQL:
SELECT MIN(presupuesto) AS min_presupuesto
FROM departamento;
```
### 4. Calcula el nombre del departamento y el presupuesto que tiene asignado, del departamento con menor presupuesto.
```SQL:
SELECT nombre, presupuesto
FROM departamento
WHERE presupuesto = (SELECT MIN(presupuesto) FROM departamento);
```
### 5. Calcula el valor máximo del presupuesto de todos los departamentos.
```SQL:
SELECT MAX(presupuesto) AS max_presupuesto
FROM departamento;
```
### 6. Calcula el nombre del departamento y el presupuesto que tiene asignado, del departamento con mayor presupuesto.
```SQL:
SELECT nombre, presupuesto
FROM departamento
WHERE presupuesto = (SELECT MAX(presupuesto) FROM departamento);
```
### 7. Calcula el número total de empleados que hay en la tabla empleado.
```SQL:
SELECT COUNT(*) AS total_empleados
FROM empleado;
```
### 8. Calcula el número de empleados que no tienen NULL en su segundo apellido.
```SQL:
SELECT COUNT(*) AS empleados_sin_null_apellido2
FROM empleado
WHERE apellido2 IS NOT NULL;
```
### 9. Calcula el número de empleados que hay en cada departamento. Tienes que devolver dos columnas, una con el nombre del departamento y otra con el número de empleados que tiene asignados.
```SQL:
SELECT d.nombre AS nombre_departamento, COUNT(*) AS num_empleados
FROM departamento d
JOIN empleado e ON d.codigo = e.codigo_departamento
GROUP BY d.nombre;
```
### 10. Calcula el nombre de los departamentos que tienen más de 2 empleados. El resultado debe tener dos columnas, una con el nombre del departamento y otra con el número de empleados que tiene asignados.
```SQL:
SELECT d.nombre AS nombre_departamento, COUNT(*) AS num_empleados
FROM departamento d
JOIN empleado e ON d.codigo = e.codigo_departamento
GROUP BY d.nombre
HAVING COUNT(*) > 2;
```

### 11. Calcula el número de empleados que trabajan en cada uno de los departamentos. El resultado de esta consulta también tiene que incluir aquellos departamentos que no tienen ningún empleado asociado.
```SQL:
SELECT d.nombre AS nombre_departamento, COUNT(e.codigo) AS num_empleados
FROM departamento d
LEFT JOIN empleado e ON d.codigo = e.codigo_departamento
GROUP BY d.nombre;
```

### 12. Calcula el número de empleados que trabajan en cada unos de los departamentos que tienen un presupuesto mayor a 200000 euros.
```SQL:
SELECT d.nombre AS nombre_departamento, COUNT(e.codigo) AS num_empleados
FROM departamento d
JOIN empleado e ON d.codigo = e.codigo_departamento
WHERE d.presupuesto > 200000
GROUP BY d.nombre;
```