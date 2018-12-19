/*this is SQL, not SAS*/
/* import file */
%LET location=/C:/; 
%LET file= BioEduJob.xlsx       ; /*Enter the filename */
 
 
PROC IMPORT 
     OUT= Bio
    DATAFILE= "&location.&file." 
    DBMS=xlsx REPLACE;
     Sheets=Bio;
    GETNAMES=YES;
RUN;
 
 
 
/*1. Join Bio and Education tabs together.*/
proc sql;
create table answer1 as
select 
* 
from Bio a 
Left JOIN Education b
on a.ID=b.'I.D.'n
;
 
/*2. Join the resultant dataset from above question(1) and join it with the Job such that everyone is associated with 
their most current job. Also, first you have to create a new dataset to join the two jobs dataset.*/
     /*proc sql;*/
     /*create table answer2a as*/
     /*select */
     /** */
     /*from Job a Left join 'Jobs 2'n b*/
     /*on a.Identification=b.Identification ;*/
 
proc sql;
create table answer2b as 
select 
c.ID, c.'First Name'n, d.Company, d.'Start Date'n
from answer1 c 
Left JOIN (select * 
     from Job a Left join 'Jobs 2'n b
     on a.Identification=b.Identification) d
on c.ID=d.Identification;
 
proc sql;
create table answer2c as 
select 
distinct * from answer2b
group by ID
having 'Start Date'n = max('Start Date'n)
;
 
/*put n/a to those jobless*/
proc sql;
create table answer2d as 
select 
     *  ,
     case 
           when Company is NULL then 'NA'
           else Company
     End as Company1 
 
from answer2c ; 
 
/*3. Join the first and last name into a single column called 'Name'. Meke sure to remove spaces*/
proc sql;
create table answer3 as
select 
CATX(' ','First Name'n, 'Last Name'n) as Name ,
('First Name'n || 'Last Name'n) as Name2 /*trying new way*/
 
from Bio 
;
 
/*4. Extract the year form the DOB column and name it 'Year of Birth'*/
proc sql;
create table answer4 as
select 
*, year(DOB) as 'Year of Birth'n
from Bio
;
 
/*5. Convert the height in cm to feet and inches. 
The new column ('Height in feet and Inches') should be like -> 5 ft and 4 inches*/
proc sql;
create table answer5a as
select 
     *, 
     Floor(Height*0.0328084) as Ft, 
     ((Height*0.0328084)-Floor(Height*0.0328084)) as Deci
from Bio;
 
proc sql;
create table answer5b as 
select 
     *,
     round(Deci,0.1)*10 as in 
from answer5a;
 
proc sql;
create table answer5c as
select 
     'First Name'n, Height, 
     CATX(' ', Ft, "'", in, "''") as Height2
from answer5b;
 
/*6. Create a new column to convert weight into pounds.*/
     /*same as above*/
 
/*7. Create a new column called 'Unique Ref Num' which is mde up of first digits of forst and last name, -, ddmm from DOB, 
eg for Lin Lan (ll-0111)*/
proc sql;
create table answer7a as
select 
*,
substr('First Name'n, 1,1) as F,
substr('Last Name'n, 1,1) as L,
month(DOB) as mm,
day(DOB) as dd
from Bio
;
THIS IS SQL
proc sql;
create table answer7b as 
select 
'First Name'n, 
CATX('', F, L, '-', dd, mm) as 'Unique Ref Num'n
from answer7a
;
 
/*8. Calculate age as of today for everyone. (Hint: Use Date Macro)*/
proc sql;
create table answer8a as
select 
*,
today() format MMDDYY10. as Today
from Bio
;
 
proc sql;
create table answer8b as
select 
*,
intck('Year',DOB,Today()) as Age 
/*directly put Today() numeric value will work, meaning argument 1 and 2 no need to be same data format*/
from answer8a
;
 
/*9. Convert height column into character, weight into number, salary into dollar amount and dates into format mmddyy10.*/
proc sql;
create table answer9a as
select 
*,
put(Height,3.) as H_Char
/*number to character*/
from Bio
;
                
proc sql;
create table answer9b as
select 
*,
input(H_Char,z3.) as H_Num
/*character to number*/
from answer9a
;
 
proc sql;
create table answer9c as
select 
*,
Salary format dollar10.1 as SalaryDollar 
/*salary into dollar amount*/
from Job
;
     
proc sql;
create table answer9d as
select
*,
DOB format mmddyy10. as DOB2,
DOB format date9. as DOB3
/*date into formats*/
from Bio
;
 
/*10. Create a column which tells how many times vowels(a,e,i,o,u) comes in everyone's names.*/
proc sql;
create table answer10a as
select
*,
countc('First Name'n, 'a') as CountA,
countc('First Name'n, 'e') as CountE,
countc('First Name'n, 'i') as CountI,
countc('First Name'n, 'o') as CountO,
countc('First Name'n, 'u') as CountU
from Bio
;
 
proc sql;
create table answer10b as
select 
*,
(CountA+CountE+CountI+CountO+CountU) as CountVowels
from answer10a
;
 
     /*use index, find the position of 'a' in each first name*/
     proc sql;
     create table answer10c as
     select 
     *,
     index('First Name'n, 'a') as position
     from Bio
     ;
 
 
/*11. Add 6 months to everyone's DOB and create a new column, and 6 days*/
proc sql;
create table answer11 as 
select 
     *,
     intnx('day',DOB,6) as Day_add FORMAT MMDDYY10.,
     intnx('month',DOB,6) as Month_add FORMAT MMDDYY10.
from Bio
;
 
/*12. Create a new column, Adult, where if age is above 18 and salary above 55000, then Y else N.*/
proc sql;
create table answer12a as 
select 
*,
intck('Year',a.DOB,Today()) as Age 
from Bio a Left Join Job b 
     on a.ID=b.Identification;
 
proc sql;
create table answer12b as
select 
'First Name'n, Age, Salary,
     Case 
           when Age>=18 and Salary >=55000 then 'Y'
           Else 'N'
     End as Adult
 from answer12a;
 
 /*13. Whcih person has the Max salary (do not use the max function, use if-else and retain)*/
 data answer13; 
 set answer12a end=LastOne;
 retain holdName holdSalary; /*Create placeholder*/
     if Salary>holdSalary then
           do;
               holdSalary=Salary;
               holdName='First Name'n;
           end;
     if LastOne;
 run;
 
 /*use proc sql*/
     proc sql;
     create table answer13b as 
     *
     from answer12a
     having salary=max(salary)
     ;
  
  /*14.Add a row number to the dataset*/
  data answer14;
  rownum=_n_;
  run;
  
  /*15. For all those people who have jobs, what was the % increase in their salary from their previous job (use both lead and lag)*/
  proc sql;
  create table answer15a as
  select
  *
  from answer12a
  where Salary is not missing 
  order by ID, 'Start Date'n asc; 
  
  DATA answer15b;
  set answer15a (drop=sex, height weight identification age);
  Salary_lag=lag(Salary);
  Increase_Percentage = (Salary - Salary_lag)/Salary_lag;
  /*then I need to remove the first obs of each group*/
 run; 
 
 
