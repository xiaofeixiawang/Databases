Q1  (1/1 point)
Return the area of Mongolia. 
```
doc("countries.xml")//country[@name="Mongolia"]/data(@area)
```
Q2  (1/1 point)
Return the names of all cities that have the same name as the country in which they are located. 
```
doc("countries.xml")//country/city[name=parent::country/@name]/name
```
Q3  (1/1 point)
Return the average population of Russian-speaking countries. 
```
doc("countries.xml")/avg(//country[language="Russian"]/data(@population))
```
Q4  (1/1 point)
Return the names of all countries that have at least three cities with population greater than 3 million. 
```
doc("countries.xml")//country[count(city[population>3000000])>=3]/data(@name)
```
Q5  (1/1 point)
Create a list of French-speaking and German-speaking countries. 
```
<result>
    <French>
    {for $c in doc("countries.xml")/countries/country
    where $c/language="French"
    return <country>{$c/data(@name)}</country>}
    </French>
    <German>
    {for $c in doc("countries.xml")/countries/country
    where $c/language="German"
    return <country>{$c/data(@name)}</country>}
    </German>
</result>
```
Q6  (1/1 point)
Return the countries with the highest and lowest population densities. Note that because the "/" operator has its own meaning in XPath and XQuery, the division operator is infix "div". To compute population density use "(@population div @area)". You can assume density values are unique. 
```
let $a := max(doc("countries.xml")//country/(@population div @area))
let $b := min(doc("countries.xml")//country/(@population div @area))
return <result>
<highest density="{$a}">
{for $c in doc("countries.xml")//country
where $c/(@population div @area)=$a
return $c/data(@name)}
</highest>
<lowest density="{$b}">
{for $c in doc("countries.xml")//country
where $c/(@population div @area)=$b
return $c/data(@name)}
</lowest>
</result>
```