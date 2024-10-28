
# Lista descendente de cada tipo de auto
SELECT 
  make,
  COUNT(*) AS tot
FROM `proj1-409300.automobile_data.car_info`
GROUP BY make ORDER BY tot DESC



# maximo y minimo de una columna
SELECT 
  MIN(length) AS valMin,
  MAX(length) AS valMax
FROM `proj1-409300.automobile_data.car_info`



# valores de puertas nulos
SELECT 
  num_of_doors,
FROM `proj1-409300.automobile_data.car_info`
WHERE
  num_of_doors IS NULL;



# valores de mazda y dodge sedan
SELECT 
  *
FROM `proj1-409300.automobile_data.car_info`
WHERE
  #num_of_doors IS NULL;
  body_style ="sedan" AND make ="dodge" OR make="mazda" 
ORDER BY make



# cambiar valores nulos a four
UPDATE automobile_data.car_info 
SET num_of_doors = "four" 
WHERE make = "dodge" AND fuel_type = "gas" AND body_style = "sedan";



# sustituir 
UPDATE cars.car_info 
SET num_of_cylinders = "two" 
WHERE num_of_cylinders = "tow";



# sustituir 
UPDATE
  automobile_data.car_info
SET
  compression_ratio=23
WHERE 
	caracteristicas


# Eliminar una fila
DELETE cars.car_info
WHERE compression_ratio = 70;



# Longitud de caracteres
/*SELECT
  drive_wheels,
  LENGTH(drive_wheels) AS ncara
FROM
  automobile_data.car_info
ORDER BY ncara DESC
*/
# DIstintoz
SELECT
  DISTINCT TRIM(drive_wheels),
FROM
  automobile_data.car_info



#typecast a float
SELECT CAST(purchase_price AS FLOAT64)
FROM `proj1-409300.customer_lauren.customer_purchase`
ORDER BY CAST(purchase_price AS FLOAT64) DESC



#Contar en brasil
SELECT 
	COUNT(Country) as NC
FROM
	WORLD
WHERE
	Country="Brazil"



#Ordenar el genero
SELECT 
  *
FROM `proj1-409300.movie_data.movies`
ORDER BY
  G__nero ASC



#Genero comedia mas antiguas
SELECT 
  *
FROM `proj1-409300.movie_data.movies`
WHERE
  G__nero="Comedia"
ORDER BY
  Fecha_de_estreno DESC



#Genero y mas de 200M
SELECT 
  *
FROM `proj1-409300.movie_data.movies`
WHERE
  G__nero="Comedia"
ORDER BY
  Fecha_de_estreno DESC



# canciones con Chris Cornell en orden descendente 
SELECT * 
FROM Tracks
WHERE 
Composer="Chris Cornell"
ORDER BY
GenreId DESC





SELECT
    COUNT(*)
FROM
    table_1
INNER JOIN
    table_2
ON table_1.key = table_2.key;

-- Using LEFT JOIN --> We get 100 rows in our results.
SELECT
    COUNT(*)
FROM
    table_1
LEFT JOIN
    table_2
ON table_1.key = table_2.key;






SELECT  
`proj1-409300.employee_data.employee`.nombre,
`proj1-409300.employee_data.employee`.puesto,
`proj1-409300.employee_data.departments`.nombre

FROM `proj1-409300.employee_data.employee`
LEFT JOIN
  `proj1-409300.employee_data.departments`

  ON
  `proj1-409300.employee_data.employee`.id_departamento = `proj1-409300.employee_data.employee`.id_departamento





SELECT 
    edu.country_name,
    summary.country_code,
    edu.value
FROM 
    bigquery-public-data.world_bank_intl_education.international_education AS edu
INNER JOIN 
    bigquery-public-data.world_bank_intl_education.country_summary AS summary
ON edu.country_code = summary.country_code






SELECT  
  orders.*,
  warehouse.alias_deposito,
  warehouse.estado
FROM `proj1-409300.warehouse_orders.orders` AS orders
JOIN
  `proj1-409300.warehouse_orders.warehouse` AS warehouse
ON  orders.id_deposito = warehouse.id_deposito








SELECT  
  COUNT(DISTINCT warehouse.estado) AS num_estado
FROM `proj1-409300.warehouse_orders.orders` AS orders
JOIN
  `proj1-409300.warehouse_orders.warehouse` AS warehouse
ON  orders.id_deposito = warehouse.id_deposito





SELECT  
warehouse.estado AS estado,
  COUNT(DISTINCT id_pedido) AS num_orders
FROM `proj1-409300.warehouse_orders.orders` AS orders
JOIN
  `proj1-409300.warehouse_orders.warehouse` AS warehouse
ON  orders.id_deposito = warehouse.id_deposito
GROUP BY
  warehouse.estado;



#ejemplo Subquery
SELECT 
  station_id,
  num_bikes_available,
  (SELECT 
    AVG(num_bikes_available)
    FROM `bigquery-public-data.new_york_citibike.citibike_stations` 
  ) AS avg_bi_av
FROM `bigquery-public-data.new_york_citibike.citibike_stations` 



#columna procentaje
SELECT 
  Date,
  Region,
  Small_Bags,
  Large_Bags,
  XLarge_Bags,
(Small_Bags*100)/Total_Bags AS Perc_SBags
FROM `proj1-409300.aguaavocado.avocado` 
WHERE
  Total_Bags<>0




#extraer dia, y contarlos
SELECT  
  EXTRACT(DAY FROM last_reported) AS day,
  COUNT(*) AS nu
FROM `bigquery-public-data.new_york_citibike.citibike_stations` 
GROUP BY
  day
ORDER BY
  day
  




#promedio de viajes en estaion con A
SELECT  
  station_name,
  ridership_2016,
  ridership_2017,
  ridership_2018,
  (ridership_2018+ridership_2017+ridership_2016)/3 AS avg_risdership
FROM 
  bigquery-public-data.new_york_subway.subway_ridership_2013_present
WHERE
  #station_name = 'Astor Pl'
  station_name LIKE 'A%'





#cantidad total vendida para cada ProductId agrupado por mes y año en que fue vendido: 
SELECT 
  EXTRACT(YEAR FROM date) AS Year, 
  EXTRACT(MONTH FROM date) AS Month, 
  ProductId, 
  ROUND(MAX(UnitPrice),2) AS UnitPrice, 
  SUM(Quantity) AS UnitsSold 
FROM 
  `proj1-409300.Empresa.sales`  
GROUP BY 
  Year, Month, ProductId 
ORDER BY 
  Year, Month, ProductId;





## estacion wicj longest bikeshare ride started
WITH longest_used_bike AS(
  SELECT
    bike_id,
    SUM(duration_minutes) AS trip_duration
  FROM
    bigquery-public-data.austin_bikeshare.bikeshare_trips
  GROUP BY
    bike_id
  ORDER BY
    trip_duration DESC
  LIMIT 1
)

SELECT
  trips.start_station_id,
  COUNT(*) AS trip_ct
FROM
  longest_used_bike AS longest
INNER JOIN
  bigquery-public-data.austin_bikeshare.bikeshare_trips AS trips
ON longest.bike_id=trips.bike_id
GROUP BY 
  trips.start_station_id
ORDER BY
  trip_ct DESC




WITH longest_used_bike AS(
  SELECT
    bike_id,
    SUM(duration_minutes) AS trip_duration
  FROM
    bigquery-public-data.austin_bikeshare.bikeshare_trips
  GROUP BY
    bike_id
  ORDER BY
    trip_duration DESC
  LIMIT 1
)

SELECT
  trips.start_station_id,
  COUNT(*) AS trip_ct
FROM
  longest_used_bike AS longest
FULL JOIN
  bigquery-public-data.austin_bikeshare.bikeshare_trips AS trips
ON longest.bike_id=trips.bike_id
GROUP BY 
  trips.start_station_id
ORDER BY
  trip_ct DESC






