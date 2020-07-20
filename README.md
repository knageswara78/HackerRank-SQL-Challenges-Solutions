
Hacker Rank SQL problem solutions for Oracle.

-------------------------------------------------------------------------------------------------------------------------------------------------------

Basic Select

1.

https://www.hackerrank.com/challenges/revising-the-select-query/problem

i.
SELECT * FROM city WHERE CountryCode = "USA" and population > 100000
SQL command not properly ended

ii. Added semi colon at end.
SELECT * FROM city WHERE CountryCode = "USA" and population > 100000;
"USA": invalid identifier

iii. USA double quotes changed to single quote.
SELECT * FROM city WHERE CountryCode = 'USA' and population > 100000;

-------------------------------------------------------------------------------------------------------------------------------------------------------

2.

https://www.hackerrank.com/challenges/revising-the-select-query-2/problem

SELECT NAME FROM city WHERE CountryCode = 'USA' and population > 120000;

-------------------------------------------------------------------------------------------------------------------------------------------------------

3.
https://www.hackerrank.com/challenges/select-all-sql/problem

SELECT * FROM city;

-------------------------------------------------------------------------------------------------------------------------------------------------------

4.

https://www.hackerrank.com/challenges/select-by-id/problem

SELECT * FROM city WHERE id = 1661;

-------------------------------------------------------------------------------------------------------------------------------------------------------


5.

https://www.hackerrank.com/challenges/japanese-cities-attributes/problem

SELECT * FROM city WHERE COUNTRYCODE  = 'JPN';

-------------------------------------------------------------------------------------------------------------------------------------------------------


6.
https://www.hackerrank.com/challenges/japanese-cities-name/problem

SELECT NAME FROM city WHERE COUNTRYCODE  = 'JPN';

-------------------------------------------------------------------------------------------------------------------------------------------------------


7.
https://www.hackerrank.com/challenges/weather-observation-station-1/problem?h_r=next-challenge&h_v=zen

SELECT CITY, STATE from STATION;

-------------------------------------------------------------------------------------------------------------------------------------------------------


8.

https://www.hackerrank.com/challenges/weather-observation-station-3/problem

Query a list of CITY names from STATION for cities that have an even ID number. 
Print the results in any order, but exclude duplicates from the answer.


mysql ok:
SELECT distinct CITY from STATION WHERE Id % 2 = 0;

oracle:
SELECT distinct CITY from STATION WHERE Id % 2 = 0;

invalid character

Replaced mysql % with MOD to solve in oracle:

SELECT distinct CITY from STATION WHERE MOD(Id,2) = 0;

or
select distinct city from station where mod(id,2) = 0;
-------------------------------------------------------------------------------------------------------------------------------------------------------

9

https://www.hackerrank.com/challenges/weather-observation-station-4/problem

Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.


SELECT count(CITY) - count(distinct CITY) from STATION;


-------------------------------------------------------------------------------------------------------------------------------------------------------

10 Very Difficult in n Oracle.

Weather Observation Station 5
https://www.hackerrank.com/challenges/weather-observation-station-5/problem

Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths 
(i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

mysql ok:

select city,length(city) 
from station
order by length(city) desc,city limit 1;
select city,length(city) 
from station
order by length(city) asc,city limit 1;

oracle:

order by length(city) desc,city limit 1
*
ORA-00933: SQL command not properly ended

order by length(city) asc,city limit 1
*
ORA-00933: SQL command not properly ended

New solution  1:
select city,length(city) from 
(select city,rownum as rid from 
 (select city,length(city) from station order by length(city),city)) 
  where rid = 1 or rid = (select max(rownum) from station);
 
New solution 2: 
select t1.* from (select City, length(City) from station order by length(city) desc, city) t1 where rownum <= 1 
union 
select t2.* from (select City, length(City) from station order by length(city) , city) t2 where rownum <= 1;  

or Instead  <= 1 ,used = 1 at end.

select t1.* from (select City, length(City) from station order by length(city) desc, city) t1 where rownum = 1 
union 
select t2.* from (select City, length(City) from station order by length(city) , city) t2 where rownum = 1;

--------------------------------------------------------------------------------------------------------------------------------------------------

11 Difficult. Use substr in oracle along with lower or upper inside substr.

https://www.hackerrank.com/challenges/weather-observation-station-6/problem


Weather Observation Station 6

Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.


Note worked:
select distinct city from station
where city like 'a%' or 'e%' or 'i%' or 'o%' or 'u%';

mysql:
worked:
select distinct city from station where city like 'a%' or city like 'e%' or city like 'i%' or city like 'o%' or city like 'u%';
or

select distinct city from station where substring(city,1,1) in ('a','e','i','o','u'); 
or

select distinct city from station where city regexp '^[aeiou].*';


Oracle:
no:
select distinct city from station where city like 'a%' or city like 'e%' or city like 'i%' or city like 'o%' or city like 'u%';

no:
select distinct city from station where substring(city,1,1) in ('a','e','i','o','u'); 

"SUBSTRING": invalid identifier

Note: It should be substr not substring.

no:
select distinct city from station where city regexp '^[aeiou].*';

invalid relational operator

Oracle WORKED:
select distinct city from station where substr(lower(city),1,1) in ('a','e','i','o','u'); 
or 
select DISTINCT CITY from STATION where SUBSTR(UPPER(CITY),1,1) IN ('A','E','I','O','U');

-------------------------------------------------------------------------------------------------------------------------------------------------------

12 Difficult. 

Weather Observation Station 7

Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

select distinct city from station where substr(lower(city),-1) in ('a','e','i','o','u');
