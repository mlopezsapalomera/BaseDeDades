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

```
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


Exemple: SELECT spIncrement(124,10) obtindriem -> 1100


Exercici 4 - Fes una funció anomenada spPringat, tal que li passem un codi de
departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.


Exercici 5 - Utilitzant la funció spPringat fes una consulta per obtenir de cada
departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.


Exercici 6 - Fes una funció anomenada spCategoria, tal que donat un codi d’empleat,
ens digui en quina categoria professional està. El criteri que volem seguir per determinar
la categoria professional és en funció dels anys que porta treballant a l’empresa:

Entre 0 i 1 anys -> Auxiliar
Entre 2 i 10 anys -> Oficial de Segona
Entre 11 i 20 Anys -> Oficial de Primera
Més de 20 anys -> Que es jubili!


Exercici 7 - Fes una consulta utilitzant la funció anterior perquè mostri mostri de cada
empleat, el codi d’empleat, el nom, els anys treballats i la categoria professional a la que
pertany.

*/



