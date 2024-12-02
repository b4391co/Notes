<h1 style="text-align:center;font-size:40px;">XBD</h1>

```sql
CREATE                  CREATE TABLE nome ( );
DROP
ALTER
DESC
Number → 3 cifras = Number(3) -999 a 999 // 3 cifras 2 decimas = Number (3,2) 1,23
Varchar
Date
Char → DNI = char(9)
```

| Clientes |  | Telefono |
| --- | --- | --- |
| Cod [PK] | <---> | Cod_C [PK] |
| nome[NN][U] |  | Telf |
| idade |

```sql
Create table CLIENTES (
    Cod number,
    Nome varchar2 (64), # Reduce tamaño
    Idade number
);
```

CONSTRAINTS = [PK][NN][U]

```sql
Create table Clientes (                     Create table Clientes (
    Cod number PRIMARY KEY,                     cod number
    Nome varchar2 (64) NOT NULL UNIQUE,         nome varchar2 (64) NOT NULL
    Idade number                                idade number,
);                                              PRIMARY KEY(cod),
                                                UNIQUE(nome)
                                            );

                    CONSTRAINT “nome” “tipo” (campo,campo2,...)
                        Create table Clientes (
                        Cod number,
                        Nome varchar2 (64) NOT NULL,
                        Idade number,
                        Constraint cli_PK Primary Key (cod),
                        Constraint cli_U Unique (nome)
                    );
```

```sql
Create table Teléfono (
    Cod_c number,
    Telf varchar2 (32),
    Constraint telf_PK Primary Key (cod_telf),
    Constraint tel_FL Foreign Key (Cod_c) REFERENCES clientes (cod)
)
    <==>
    Create table Teléfonos (
    Cod_c number,
    Telf varchar2 (32),
    Constraint telf_PK Primary Key (cod_telf)
)
Alter table Teléfonos
    ADD Constraint tel_FL Foreign Key (Cod_c) REFERENCES clientes (cod);
DROP table Clientes
CASCADE constraints;
```

DESC = da los campos de la tabla
ALTER = Alterar tabla
DROP = Borrar columna
RENAME = renombrar columna
DESC nome;
Alter Table nome

* Modify Idade number;
* Drop column nome_columna cascade constraints;
* Rename column nome_columna TO nuevo_nombre,

| . | . | . |
| --- | --- | --- |
| CREATE | INSERT | Tuplas |
| DROP | DELETE | Tuplas |
| ALTER | UPDATE | Campo |
| DESCRIBE | SELECT | |

```sql
Insert into clientes (DNI, nome, enderzo, email)
Values (1,’Pepe’,’rua de ...’,’pepe@gmail.com’) ;

Delete from clientes where [cod =3];

Update clientes set campo=valor where [condición];
Select campo1,campo2,       Select nombre, apellido
    From tabla              from empleados
    Where [Condición];      where departamento=10;

NOT (idade=20) ⇔ idade <> 20
NOT(idade=20 AND dep_id<>21) ⇔ NOT idade=20 OR dep_id=21
(idade=20 OR idade=30) AND cidade=’Madrid’ ⇔ where idade IN(20,30,40) AND
cidade=’x’;
idade>=18 AND idade <=80 ⇔ idade between 18 AND 80
Select... From... Where...
ORDER BY campo1, campo2 DESC,
                            ASC;

SELECT DISTINCT campo1,campo2

Sum ( x )	Avg ( x )   Count( x )

Select avg(salario) As MEDIAA # ← Media con el nombre “MEDIAA”

From empleados;
```

1. Cuantas filas tienen valor en un campo:

```sql
Select count(Categoria) From productos  ← Cuenta cuántas Categorías existen Not null
```

2. Todas las filas independientes de los nulos

```sql
Select Count(*) from productos;
```

3. Distintos Valores

```sql
Select Count(Distinct categoria) from producto;
```

## GROUP BY categoria

1. Si seleccionamos un atributo con una función de agregación ese atributo va en el GROUP BY

```sql
Select Count(*), Precio From Productos Group by precio;
```

2. 
3. Las condiciones sobre los campos van en el WHERE y las condiciones sobre las funciones de agregación van en el HAVING

```sql
Select dep,Sum(salario),Precio From empleados Group by dep Having sum(salario)>2000
```

4. Si hay Having hay Group by
5. Cuando la función de agregación está a la izquierda de la condición va en un HAVING.

```sql
Select max(sueldo)

Cuando está a la derecha hay que hacer un nuevo SELECT
Where
= ( Select * from…)
Having

Select dep from empleados Group by dep Having Avg(sueldo) = (select avg(sueldo) from empleados)

Select * from empleados where sueldo = (select max(sueldo) from empleados)
```

### BASE

```sql
Select X Group by X having count(*) = Y,`
```

## CROSS JOIN

```sql
Select nome, cod
From clientes,pedidos
where cliente=DNI;

Select clientes.nome, pedidos.cod
From clientes,pedidos
where clientes.DNI=pedidos. DNI;

Select a.nome, p.cod
From clientes a,pedidos p
where a.DNI=p.DNI
Group by a.nome
;
```

### CROSS JOIN

1. Seleccionar campos de dos tablas (obligatorio)
2. Seleccionar campos de una tabla con condicioones en otra

## SUBCONSULTAS

> Quiero seleccionar algo en una tabla y la condición se establece en otra

1. Son obligatorias con deletes e updates.
2. NO poner 2 atrib en el where si estamos pidiendo 1 cosa

```sql
Select Importe
from pedidos
Where cliente In
(Select DNI from clientes where dni=’x’);
```

> Borrar pedidos prov nom = coruña

```Select a.cod
from pedidos a,cliente b,provincia c
where a.cliente=b.dni and b.provinica=c.cod
and c.nome=’coruña’;

Delete from pedidos where cliente in
(select dni from clientes where  provincia in
(select cod from provincias where nome=’coruña’));
```

> MAX soldo de cada departamento:

```sql
Select * from trabajadores t where soldo in
(select max(soldo) from trabajadores where dep=t.dep);
```

> Nome dep xunto nome trabajadores

## CROSS

```sql
Select a.nome,b.nome as trab from departamentos a,traballadores b
Where a.cod=b.dep and b.soldo>1000;
```

## INNER

```sql
Select a.nome,b.nome as trab from departamentos a join traballadores b
ON a.cod=b.dep where b.soldo>1000;
```

## UNION:

### INTERSECT:

1. Valores comunes entre 2 selects
   EXCEPT / MINUS:

### ESTÁ EN:

#### Subconsulta:

```sql
Select nome from departamentos where cod IN (select dep from traballadores);
```

#### Join:

```sql
Select distinct a.nome from departamentos a, traballa b  where a.cod=b.dep;
```

#### Exist:

```sql
Select nome from departamentos where EXIST (select * from traball where dep=t.cod);
```

> CUANDO HAGO UN JOIN SIEMPRE SALEN LOS DATOS DEL LADO MUCHOS.
> 
> EN EL JOIN SALEN LAS FILAS DEL LADO MUCHOS SI LA FK TIENE VALOR, SINO NO SALE.

### NO ESTÁ EN:

#### Subconsulta:

```sql
Select nome from departamentos where cod NOT IN (select dep from traballadores
where dep is not null);
```

> Join: No se puede:
> Exist:

```sql
Select nome from departamentos where NOT EXIST (select * from traball where dep=t.cod);
```

### OUTER JOIN:

> Listado de todos los trabajadores junto con el nombre del departamento

```sql
Select a.nombre,a.sueldo,b.nombre as depart
From trabajadores a left JOIN departamento b
ON a.dep=b.cod

Select b.nombre,count(*) as num
From trabajadores a left JOIN departamento b
ON a.dep=b.cod
Group by b.cod,b.nombre;
```

### LEFT // RIGHT // FULL

> COUNT (*)
> Obtener not null
> Obtener distintos valores de un campo
> En los outer join

```sql
Select b.nombre,count(a.DNI) as num,(sum(NVL(sueldo,0)) as gasto
From trabajadores a right JOIN departamento b
ON a.dep=b.codn
Group by b.cod,b.nombre;

Where sueldo > ALL (select…)
(1000,2000,4000);
```

## SELECTS ANIDADOS:

> `Rownum` (metacampo que se puede seleccionar y devuelve el número de fila por el que   va )

```sql
Select rownum ,dni,nome from clientes where rownum < 6;
```

1. Solo funciona = x		< x  		<= x

```sql
Select cod from
( Select rownum,cod from
( Select cod from productos order by prezo ))
Where fila > 4 and fila < 8

Select * from (
Select rownum as fila, first_name,salary from(
Select frist_name,salary from employees
Order by salary des) where rownum=2)
Where fila=2;
```

### FUNCIONES NUMERICAS:

```sql
Select abs (cant) from…   Seleccionas un campo numérico se ordena con todo positivo
```

### VISTAS

```sql
Create VIEW nome as Select…

Create view depart as select a.cod, a.nom, sum(b.salario) as presupuesto
From departamento a, empregados b
Where a.cod=b.dep group by  a.cod,a.nom;

Select sum(presupuesto) from depart;
```

> Butacas:

| col1 |
| --- | 
| Butacas |  
| Num [PK] |
| dni |

> Cuando se envie el dni se envia:

```sql
$numero=1
$dni=123456789A

Select * from butacas; ← pintar las butacas
Update butacas set dni=$dni where num=$numero;
```

**Bloqueos**

- Lock table
- select for update
- transicion
  - insert
  - update
  - delete

deadlock

> LISTADO COMPLETO = OUTER JOIN

#### PL - SQL

> Variable 3*2
> -declarar:	nombre   tipo
> Ex,	edad number		int edad
> Int i=6
> -asignarle valor: 	nombre := valor		edad=2*3
> Edad := 2*3

## Funciones

### Crear FUNCION

```sql
Create function suma( num1 number, num2 number )
return number
As		
*Declarar variables*
S number;
Begin		
*Codigo*
S := num1+num2;
Return S;
End;
```

### Llamar a la FUNCION

1. Ponerlo en un select

```sql
Select suma (3,2) from DUAL (tabla que no existe);
Select 5 from dual;
```

2. En cualquier sitio que admita un valor

```sql
Idade := 3* sum(8,10);
```

### SELECT … INTO

```sql
Select Campo INTO Variable From …
```

> 1. El tipo de variable = tipo de campo
> 2. El select SIEMPRE tiene que devolver un valor
> 3. El select SOLO puede devolver un valor
>    Si el SELECT no devuelve ninguna tupla o más de una lanza una excepción

> 1. EJECUTAN EN ORDEN
> 2. DECISION

```sql
IF CONDICION THEN {
	Se ejecuta la condición si es TRUE
	}
ELSE {
	}
END IF;
```

> SI EL SELECT DEVUELVE MUCHAS FILAS USAR CURSOR

## CURSOR

### DECLARAR CURSOR

```sql
Cursor nome is select …
```

### ABRIR CURSOR

```sql
Open nome;
```

### RECORRER CURSOR (BUCLES)

#### WHILE

```sql
While condicion loop
# ∞ hasta que cambie la condición
End loop;
```

#### FOR

```sql
Foreach
```

#### LOOP

```sql
fetch nome into variable;
```

### CERRAR CURSOR

```sql
Close nome;
```

### PROCEDIMIENTOS

```sql
DBMS_OUTPUT.PUT_LINE( ) ;
SET SERVEROUTPUT ON ;
```

### CURSORES PARAMETRIZADOS

```sql
CURSOR nome(id number) in select * from employees where employees_id=id,
BEGIN
OPEN nome,
For v_emp in nome(100) loop
```

### DECLARAR EXCEPCIONES

1º DECLARAR

```sql
ENULO EXCEPTION;
BEGIN ...						
IF IDADE IS NULL THEN			
RAISE ENULO;				
END IF;
```

## TRIGGER

1. Triggers que evitan el evento - lanzando excepción

```sql
After/Before	FES/FER
```

2. Triggers que cambian el evento

```sql
Before 	FER
```

3. Triggers que cambian datos de tablas

```sql
After/Before	FES/FER
```

```sql
Declare/Create trigger nome [before/after] [insert/update/delete] on tabla
Begin
...
End; /
```

## CONFIGURACIÓN

### CONTRASEÑA (TERMINAL)

```terminal
sqlplus sys@cdb1 as sysdba		; Pass: *****
SQL> alter user system identified by oracle;
SQL> alter altered
SQL> alter profile default limit
Password_life_time unlimited;
SQL> profile altered
```

### CREAR USUARIO ( ORACLE SQL )

```terminal
DBA > Users > new user

> user > default tablespace = users
> Granted roles > resource // connect
> quotas > users
```

### CONECTION ( SQL )

```terminal
Conection > + >
boletin1
Boletin1
oracle
Sshtunnel.XXXXXXX.com		172.20.0.13
64022					1521
Orclpdb.XXXXXXX.com		=

Orclpdb.XXXXXXX.com	172.20.0.13	1521
```
