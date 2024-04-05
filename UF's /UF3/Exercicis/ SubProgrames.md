<h1>ACTIVITAT - EXERCICIS SUBPROGRAMES</h1>
<p>Exercici 1 - Fes una funció anomenada spData, tal que donada una data en format
MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA
Exemple : SELECT spData('1988-12-01') => 01-12-1988</p>

```mysql
DELIMITER //
CREATE FUNCTION spData(data_mysql DATE)
RETURNS VARCHAR(10)
BEGIN
    DECLARE data_convertida VARCHAR(10);
    SET data_convertida = DATE_FORMAT(data_mysql, '%d-%m-%Y');
    RETURN data_convertida;
END //
DELIMITER ;
```
<p>Exercici 2 - Fes una funció anomenada spPotencia, tal que donada una base i un
exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW.</p>

<p>Exemple : SELECT spPotencia(2,3) => 8</p>

```mysql
DELIMITER //
CREATE FUNCTION spPotencia(base INT, exponent INT)
RETURNS INT
BEGIN
    DECLARE result INT DEFAULT 1;
    DECLARE i INT DEFAULT 1;

    WHILE i <= exponent DO
        SET result = result * base;
        SET i = i + 1;
    END WHILE;

    RETURN result;
END //
DELIMITER ;
```
<p>Exercici 3 - Fes una funció anomenada spIncrement que donat un codi d’empleat i un
% de increment, ens calculi el salari sumant aquest percentatge.</p>
<p>Per exemple, suposem que l’ empleat amb id_empleat = 124 té un salari de 1000</p>

<p>Exemple: SELECT spIncrement(124,10) obtindriem -> 1100</p>

```mysql
DELIMITER //
CREATE FUNCTION spIncrement(id_empleat INT, increment DECIMAL(5,2))
RETURNS DECIMAL(8,2)
BEGIN
    DECLARE salari_actual DECIMAL(8,2);
    DECLARE salari_incrementat DECIMAL(8,2);

    -- Obtener el salario actual del empleado
    SELECT salari INTO salari_actual
    FROM empleats
    WHERE empleat_id = id_empleat;

    -- Calcular el salario incrementado
    SET salari_incrementat = salari_actual + (salari_actual * (increment / 100));

    RETURN salari_incrementat;
END //
DELIMITER ;
```
<p>Exercici 4 - Fes una funció anomenada spPringat, tal que li passem un codi de
departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.</p>

```mysql
DELIMITER //
CREATE FUNCTION spPringat(codi_departament INT)
RETURNS INT
BEGIN
    DECLARE empleat_pringat_id INT;

    -- Obtener el código del empleado que gana menos en el departamento dado
    SELECT empleat_id INTO empleat_pringat_id
    FROM empleats
    WHERE departament_id = codi_departament
    ORDER BY salari ASC
    LIMIT 1;

    RETURN empleat_pringat_id;
END //
DELIMITER ;
```
<p>Exercici 5 - Utilitzant la funció spPringat fes una consulta per obtenir de cada
departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.</p>

```mysql
SELECT d.departament_id, d.nom AS nom_departament, spPringat(d.departament_id) AS empleat_pringat_id
FROM departaments d;
```
<p>Exercici 6 - Fes una funció anomenada spCategoria, tal que donat un codi d’empleat,
ens digui en quina categoria professional està. El criteri que volem seguir per determinar
la categoria professional és en funció dels anys que porta treballant a l’empresa:</p>

<p>-Entre 0 i 1 anys -> Auxiliar</p>
<p>-Entre 2 i 10 anys -> Oficial de Segona</p>
<p>-Entre 11 i 20 Anys -> Oficial de Primera</p>
<p>-Més de 20 anys -> Que es jubili!</p>

```mysql
DELIMITER //
CREATE FUNCTION spCategoria(id_empleat INT)
RETURNS VARCHAR(30)
BEGIN
    DECLARE anys_treballats INT;
    DECLARE categoria VARCHAR(30);

    -- Obtener los años que lleva trabajando el empleado
    SELECT TIMESTAMPDIFF(YEAR, data_contractacio, CURDATE()) INTO anys_treballats
    FROM empleats
    WHERE empleat_id = id_empleat;

    -- Determinar la categoría profesional según los años trabajados
    IF anys_treballats BETWEEN 0 AND 1 THEN
        SET categoria = 'Auxiliar';
    ELSEIF anys_treballats BETWEEN 2 AND 10 THEN
        SET categoria = 'Oficial de Segona';
    ELSEIF anys_treballats BETWEEN 11 AND 20 THEN
        SET categoria = 'Oficial de Primera';
    ELSE
        SET categoria = 'Que es jubili!';
    END IF;

    RETURN categoria;
END //
DELIMITER ;
```
<p>Exercici 7 - Fes una consulta utilitzant la funció anterior perquè mostri mostri de cada
empleat, el codi d’empleat, el nom, els anys treballats i la categoria professional a la que
pertany.</p>

```mysql
SELECT
    empleat_id AS codi_empleat,
    nom,
    TIMESTAMPDIFF(YEAR, data_contractacio, CURDATE()) AS anys_treballats,
    spCategoria(empleat_id) AS categoria_professional
FROM
    empleats;
```



