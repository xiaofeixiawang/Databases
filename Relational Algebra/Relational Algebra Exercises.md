Q1  (1/1 point)
Find all pizzas eaten by at least one female over the age of 20. 
```
\project_{pizza}
    (
        (\project_{name}Eats) 
        \intersect 
        (\project_{name}
            (\select_{age>20 and gender = 'female'} Person))
        \join Eats);
```
Q2  (1/1 point)
Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.) 
```
\project_{name}
    (
    \select_{gender = 'female'}
        (
        \project_{name}
        (
            \project_{pizza}
            (\select_{pizzeria = 'Straw Hat'}Serves)
            \join Eats
        )
        \join Person
        )
     );
```
Q3  (1/1 point)
Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat. 
```
\project_{pizzeria}
    (\select_{price < 10}
        (\project_{pizza}
            (\select_{name = 'Amy' or name = 'Fay'}Eats)
        \join Serves));
```
Q4  (1/1 point)
Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat. 
```
\project_{pizzeria}
    (\select_{price < 10}
        ((
        (\project_{pizza}
            (\select_{name = 'Amy'}Eats))
            \intersect
        (\project_{pizza}
            (\select_{name = 'Fay'}Eats))
        )
        \join Serves)
     );
```
Q5  (1/1 point)
Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos. 
```
(\project_{name}
    ((\project_{pizza}
        (\select_{pizzeria = 'Dominos'}Serves))
        \join Eats))
\diff
(\project_{name}
    (\select_{pizzeria = 'Dominos'}Frequents))
```
Q6  (1/1 point)
Find all pizzas that are eaten only by people younger than 24, or that cost less than $10 everywhere they're served. 
```
((\project_{pizza}
    (\project_{name}
        (\select_{age<24}Person)
    \join Eats))
\diff
(\project_{pizza}
    (\project_{name}
        (\select_{age>=24}Person)
    \join Eats))
)
\union
((\project_{pizza}
    (\select_{price<10}Serves))
\diff
(\project_{pizza}
    (\select_{price>=10}Serves))
)
```
Q7  (1/1 point)
Find the age of the oldest person (or people) who eat mushroom pizza. 
```
(\rename_{age2}
    (\project_{age}
        ((\project_{name}
            (\select_{pizza = 'mushroom'}Eats)) 
                \join Person)))
\diff
(\project_{age2}
    (
    (\rename_{age2}
        (\project_{age}
            (\select_{pizza = 'mushroom'}Eats 
                \join Person)))
        \join_{age2<age1}
    (\rename_{age1}
    (\project_{age}
    (\select_{pizza = 'mushroom'}Eats 
    \join Person)))
    ))
```
Q8  (1/1 point)
Find all pizzerias that serve only pizzas eaten by people over 30. 
```
(\project_{pizzeria}Serves)
\diff
(\project_{pizzeria}
(
((\project_{pizza}Serves)
\diff
(\project_{pizza}
(\project_{name}
    (\select_{age>30}Person)
\join Eats)
))
\join Serves)
)
```
Q9  (1/1 point)
Find all pizzerias that serve every pizza eaten by people over 30. 
```
(\project_{pizzeria}Serves) 
\diff 
    (\project_{pizzeria}((\project_{pizzeria}Serves) 
        \cross 
        (\project_{pizza}(\select_{age>'30'}Person \join Eats)) 
    \diff 
    (\project_{pizzeria,pizza}
    ((\select_{age>'30'}Person \join Eats) \join Serves))))    
```                          