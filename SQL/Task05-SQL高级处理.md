窗口函数的通用形式：

<窗口函数> OVER ([PARTITION BY <列名>]
                     ORDER BY <排序用列名>)  
*[]中的内容可以省略。
窗口函数最关键的是搞明白关键字PARTITON BY和ORDER BY的作用。

PARTITON BY是用来分组，即选择要看哪个窗口，类似于GROUP BY 子句的分组功能，但是PARTITION BY 子句并不具备GROUP BY 子句的汇总功能，并不会改变原始表中记录的行数。

ORDER BY是用来排序，即决定窗口内，是按那种规则(字段)来排序的。

窗口函数可以分为两类。

一是 将SUM、MAX、MIN等聚合函数用在窗口函数中

二是 RANK、DENSE_RANK等排序用的专用窗口函数
RANK函数
计算排序时，如果存在相同位次的记录，则会跳过之后的位次。

例）有 3 条记录排在第 1 位时：1 位、1 位、1 位、4 位……

DENSE_RANK函数
同样是计算排序，即使存在相同位次的记录，也不会跳过之后的位次。

例）有 3 条记录排在第 1 位时：1 位、1 位、1 位、2 位……

ROW_NUMBER函数
赋予唯一的连续位次。

例）有 3 条记录排在第 1 位时：1 位、2 位、3 位、4 位

语法

<窗口函数> OVER (ORDER BY <排序用列名>
                 ROWS n PRECEDING )  
                 
<窗口函数> OVER (ORDER BY <排序用列名>
                 ROWS BETWEEN n PRECEDING AND n FOLLOWING)
PRECEDING（“之前”）， 将框架指定为 “截止到之前 n 行”，加上自身行

FOLLOWING（“之后”）， 将框架指定为 “截止到之后 n 行”，加上自身行

BETWEEN 1 PRECEDING AND 1 FOLLOWING，将框架指定为 “之前1行” + “之后1行” + “自身”

5.1
根据product_id排序，计算累计到当前行的最大sale_price。
5.2
继续使用product表，计算出按照登记日期（regist_date）升序进行排列的各日期的销售单价（sale_price）的总额。排序是需要将登记日期为NULL 的“运动 T 恤”记录排在第 1 位（也就是将其看作比其他日期都早）
SELECT product_id
       ,product_name
       ,regist_date
       ,SUM(sale_price) OVER (partition by regist_date) AS sum_price_by_date
  FROM product
ORDER BY -ISNULL(regist_date), regist_date;
5.3
① 窗口函数不指定PARTITION BY的效果是什么？
回答：如果不指定PARTITION BY的话，窗口函数的操作窗口就是整个表（即所有记录都算为同一个分组）。比如rank会对指定的列进行排序，不会再分成多组分别进行排序。
② 为什么说窗口函数只能在SELECT子句中使用？实际上，在ORDER BY 子句使用系统并不会报错。
回答：窗口函数会对整个表进行分组，而 WHERE子句 和 HAVING子句 分别是放的记录和字段筛选条件、GROUP BY子句放的是分组依据，都不适合把窗口函数放到其中使用。而ORDER BY子句则是对SELECT子句中的结果进行操作，操作的是整个结果表，所以可以使用窗口函数，但是窗口函数的返回结果只作为ORDER BY子句的排序依据，并不能返回期望的结果。

