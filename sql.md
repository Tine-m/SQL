# SQL Aggregate Functions (Murach Chapter 6 Summary Queries)

An aggregate function is a computation where the values of multiple rows are grouped together as input on certain criteria to form a single value of more significant meaning.

![](img/aggregate_functions.png)

## Learning Objectives
The idea is that you after working with these exercises:

- is able to code summary queries using the aggregate functions: AVG, SUM, MIN, MAX, COUNT, COUNT(*)
- is able to code summary queries with HAVING and GROUP BY
- can maintain an SQL script to handle database changes
- AUTO_INCREMENT


## Relevance for your professional life
Pretty much all systems need to persist data and relational databases are the most widely used for (administrative) systems. You will need SQL as a query language to get data out of the database.

##  Supplementary resources
  - [w3schools tutorials: ](https://www.w3schools.com/sql/default.asp) Min, Max, Avg, Count, Sum
- [LinkedIn Learning video](https://www.linkedin.com/learning/sql-essential-training-3/what-are-aggregates?u=36836804) SQL Essential Training: chapter 8


## Why aggregate functions?
Sometimes we don't "just" want to retrive the actual rows from the database, but rather need information based on aggregated data. For instance:
* Who earns the most in the company?
* What is the total sum of salaries in the development department?
* What is the average salary based on gender?

SQL aggregate functions can give of these summary queries. There are five functions: 

### `count()`

* count(*): counts total number of rows.
* count(salary): counts number of not null values for the column, e.g. salary.
* count(distinct salary):  counts number of distinct non null values for the column, e.g. salary.

### `sum()`

* sum(salary):  sums all not null values for column, e.g. salary.
* sum(distinct salary): sums all distinct non null values for the column,e.g. salary.

### `avg()`

* avg(salary) = sum(salary) / count(salary).
* avg(distinct salary) = sum (distinct salary) / count(distinct salary).

 
### `min()` & `max()`

* min(salary): finds minimum value among the the not null values for the column, e.g. salary.
* max(salary): finds maximum value among the the not null values for the column, e.g. salary.

# Examples (together) 

Let’s use this table ‘orders’:

![](img/orders_table.png)

### Stated in natural language, what do the following SQL statements express (the result is shown below the SQL statement)?

A)
```sql
select count(*) from orders;
```
![](img/example_A.png)

B)
```sql
select count(distinct customer_id) from orders;
```
![](img/example_B.png)

C)
```sql
select min(order_date) from orders;
```
![](img/example_C.png)


# Exercises (30 minutes)

We will use the coffee-database (the database you used in Benjamin’s classes). If you don’t have the database already, find the script to create the database [here.](https://github.com/behu-kea/dat20-classes/blob/master/week-11/assets/coffee-database.sql)


1.	Find the number of female customers.
```sql
select count(*) from customer
where gender = 'F';
```

2.	Find the number of customers whose last name starts with ‘B’.
```sql
select count(*) from customer
where lastname like 'B%';
```
3.	Calculate the average price of all products (only output the average price, no other information).
```sql
select avg(price) from product;
select round(avg(price)) from product;
```

4.	List product id, country and price for all products (no aggregate function, but a join of product and country tables is needed).

```sql
select product_id, country, price from product inner join country on product.country_id = country.country_id;
```

5. Calculate the total sum of all orders.

```sql
SELECT sum(qty * price) FROM order_details natual join product;
```


### Group by and having

You can aggregate data grouped by some criteria using `GROUP BY`: <br>
In this example we are counting the number of orders for each customer (i.e grouped by customer_id):

```sql
select customer_id, count(*) from orders
group by orders.customer_id;
```
![](img/example_C.png)


You can limit the result, combining `GROUP BY` and `HAVING`. `HAVING` determines which groups are included in the final result. <br>
In this example we are counting the number of orders for each customer (i.e grouped by customer_id), BUT only for customers that have more than 2 orders:

```sql
select customer_id, count(*) from orders
group by orders.customer_id
having count(*) > 2;
```
![](img/example_E.png)

<br>
<br>
Use `GROUP BY` and `HAVING`in the following exercises.
<br>

6. List number of employees of each gender.
```sql
select count(*) from customer
group by gender;
```

7. List number of employees of each gender, BUT only if there are more than 10 employees of the gender.
```sql
select count(*) from customer
group by gender having count(gender) > 10;
```

8.	List the average price (and no other information) for products for each country.
```sql
select avg(price) from product
group by country_id;
```

9.	List product id, country name and average product price for each country (you will need to join product and country tables).
```sql
select product_id, country, avg(price) from product inner join country on product.country_id = country.country_id
group by country.country_id;
```

10. List product id, country name and average product price for each country, but only for countries that have an average product price higher than 20.00. 

```sql
select product_id, country, avg(price) from product inner join country on product.country_id = country.country_id
group by country.country_id having avg(price) > 20.0;
```

### SQL Script exercise


Something doesn’t quite seem right above the design of the coffee database.
What could that be?


![](img/coffeeDB.png)

What can we do about it?
```
$
Redesign database
1.	Update SQL script
2.	Make sure that the primary keys can be create automatically (hint: `AUOT_INCREMENT`)
3.	Run SQL script 
$
```
**Voila you now have a redesigned database, ready to be shared wth the rest of your team!**


git clone kommandoen skulle så gerne skrive noget i stil med:

```
$ git clone https://github.com/kaspercphbusiness/test100.git
Cloning into 'test100'...
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 7 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (7/7), done.
$
```


**Tillykke, du har nu dit første lokale git repository!**


#### Kopier filer ind i workspace
Du har sikkert allerede et IntelliJ projekt. Du kan nu kopiere filerne ind i det nyt git projekt og bruge `git log` og `git status` til at se, hvad der er sket ind til videre.

Der findes andre metoder til at sætte det op. Det er muligt at starte med at lave et lokalt repository, og så senere sætte det på som delt repository. Men metoden ovenfor virker.



# Opgave
Opgaven er at få sat et repository op for jeres Grocery List opgave (Java repetitionsopgave). 

Der er dog først lige nogle øvelser, der ikke handler om denne opgave.


### Opg 1
Der skal oprettes et repository på github, som I skal være to om. 

* Som led i at oprette dette repository vil jeg anbefale at I i readme filen skriver, hvilke medlemmer der er med i gruppen, og hvad jeres github brugernavne er.
* Husk at få lavet .gitignore filen - se ovenfor.
<!--- * Inviter dine gruppemedlemmer til at være med i dette repository. --->


### Opg 2
Download (ikke vha clone, blot ren gammeldags zip) [Kodestafet zipfil på Fronter](https://kea-fronter.itslearning.com/LearningToolElement/ViewLearningToolElement.aspx?LearningToolElementId=847847)  og få det til at køre. 

### Opg 3
Lave en klon af github projektet og kopierer IntelliJ projektet over i denne klon og få det op på github.

### Opg 4
Klon til den andens maskine og få det også til at køre fra dennes maskiner (I har nu to kloner kørende lokalt).

### Opg 5
Den ene af jer ændrer datoformatet i udskrivDato metoden() i klassen Hovedmenu.java så man får udskrevet dato med punktum i stedet for bindestreg. F.eks. 31.08.2020.

### Opg 6
Den, der ændrede, skal nu få disse ændringer på github. Det gøres ved:

```bash
$ git add . # Tilføj alle ændringer fra workspace til staging/index.
$ git commit -m "Ændret datoformat"
$ git push # Push ændringer til github
```
### Opg 7
Den anden skal nu få denne ændring ned på sin egen maskine

```bash
$ git pull # opdater local repository og workspace
```

### Opg 8
Slet det directory som indeholder projektet. Check at det er fuldstændigt væk, dødt, pist borte! Dernæst gå til jeres Github og klon det, så du får det igen. Pointe: Hvis det er på jeres github er det aldrig tabt!!!!

## Merge opgave (extra)
Denne opgave er optionel - men skal laves af alle på et tidpunkt senere i semesteret.

### Opg 9 
Når man skal arbejde sammen, så er der mange ting der skal falde på plads,  her vil vi se på to ting:

* Man prøver at aftale hvem der arbejder på hvilke filer (f.eks. en laver database og en anden laver web sider ...)
* Det er vigtigt at man ofte laver et `git pull` så man holder sit workspace opdateret i forhold til hvad de andre i gruppen har lavet.

Men det sker at der opstår "merge konflikter". Det sker hvis du har været inde i fil X og rettet i linje 18, samtidigt med at der er en anden der har gjort det samme. Hvis den anden har sagt `git push` før dig, så vil du få en række fejl når du siger `git push`:

* Først vil du få at vide at du skal sige pull, da du er "bagud" i forhold til github.
* Når du så siger pull, vil du få en meget lang fejlbeskrivelse. Den siger, at git ikke selv kunne finde ud af at sætte de to ændringer sammen (der er rettet det samme steder i de samme filer).

1. Få dette til at ske - den ene af jer laver en ny ændring i datoformat i udskrivDato() metoden og pusher denne rettelse til github.
2. Den anden laver også ændring i udskrivDato() metodens datoformat, men til noget andet forstås! Og prøver derefter et `git push` (efter `git add` og `git commit`).
3. Dette bør give fejl. Så skrives `git pull`.
4. Dette giver en fejlmeddelse i stil med denne:

![](img/merge2.PNG)

Åbn projektet i IntelliJ for at få vist merge konflikter. Klik på linket "Resolve":

![](img/merge3.PNG)



Du er ansvarlig (over for gruppen) for at få rettet dette til noget, der er OK. Det kan betyde du skal på github for at se hvem der har lavet den anden del, snakke med vedkommende, og så blive enige om hvad der skal gøres. Men det er dig, der skal rette det!

Når du igen har noget kode der virker (måske er der dele fra begge to, der skal bruges) så kan du lave:

`git add .` 

`git commit -m "Rettet merge konflikt"` og 

`git push`.

Du har nu håndteret din første merge konflikt.

