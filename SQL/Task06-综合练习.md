1.
select d.Name Department,e.Name Employee ,e.Salary  from Employee e
right join Department d on d.Id=e.DepartmentId
left join (select Max(Salary) Salary,DepartmentId from Employee group by Employee.DepartmentId) a
on a.DepartmentId = e.DepartmentId
where e.Salary = a.Salary

2.
select id,if(t.id%2=1,t.back,t.ahead) as result
from 
(select id,student,lag(student,1,student) over() as ahead,lead(student,1,student) over() as back
from seat) t

3.
select Score,row_number() over(order by Score desc) as Rank
from Scores

4.
select distinct f1.num as ConsecutiveNums
from Logs f1 
left join Logs f2 on f1.id = f2.id + 1
left join Logs f3 on f1.id = f3.id + 2  
where f1.num = f2.num and f1.num = f3.num;  

5.
select id,
case when t.p_id is null then 'Root' 
     when t.id in (select p_id from tree ) then 'Inner'
     else 'Leaf' 
     end as Type
from tree t 

6.
select a.Name
from Employee a
left join Employee b
on a.Id = b.ManagerId
group by a.Id
having count(1) >=  5

7.
select Score,rank() over(order by Score desc) as Rank
from Scores

8.
SELECT question_id as survey_log
FROM
(
	SELECT question_id,
		SUM(case when action = "answer" THEN 1 ELSE 0 END) as num_answers,
		SUM(case when action = "show" THEN 1 ELSE 0 END) as num_shows
	FROM survey_log
	GROUP BY question_id
) as tbl
ORDER BY (num_answers / num_shows) DESC
LIMIT 1;

9.
select Department.Name,t.Name,t.Salary from 

(select Id,Name,DepartmentId,salary,row_number() over (partition by DepartmentId order by Salary DESC) as 'rank'
from employee) t
left join Department
on t.DepartmentID = Department.id
where t.rank<=3










