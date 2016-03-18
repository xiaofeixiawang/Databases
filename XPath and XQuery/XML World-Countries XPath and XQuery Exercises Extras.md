Q1  (1/1 point)
Return the names of all countries with population greater than 100 million. 

doc("countries.xml")//country[@population>100000000]/data(@name)

Q2  (1/1 point)
Return the names of all countries where over 50% of the population speaks German. (Hint: Depending on your solution, you may want to use ".", which refers to the "current element" within an XPath expression.) 

doc("countries.xml")//country[language[.="German"]/@percentage>50]/data(@name)

Q3  (1/1 point)
Return the names of all countries where a city in that country contains more than one-third of the country's population. 

doc("countries.xml")//country[(@population div 3) < city/population]/data(@name)

Q4  (1/1 point)
Return the population density of Qatar. Note: Since the "/" operator has its own meaning in XPath and XQuery, the division operator is "div". To compute population density use "(@population div @area)". 

doc("countries.xml")//country[@name="Qatar"]/(@population div @area)

Q5  (1/1 point)
Return the names of all countries whose population is less than one thousandth that of some city (in any country). 

doc("countries.xml")//country[(@population * 1000) < doc("countries.xml")//city/population]/data(@name)

Q6  (1/1 point)
Return all city names that appear more than once, i.e., there is more than one city with that name in the data. Return only one instance of each such city name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.) 

doc("countries.xml")//city[name=preceding::city/name]/name

Q7  (1/1 point)
Return the names of all countries containing a city such that some other country has a city of the same name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.) 

doc("countries.xml")//country[city/name=preceding::name or city/name=following::name]/data(@name)

Q8  (1/1 point)
Return the names of all countries whose name textually contains a language spoken in that country. For instance, Uzbek is spoken in Uzbekistan, so return Uzbekistan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.) 

doc("countries.xml")//country[language[contains(../@name,.)]]/data(@name)

Q9  (1/1 point)
Return the names of all countries in which people speak a language whose name textually contains the name of the country. For instance, Japanese is spoken in Japan, so return Japan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.) 

doc("countries.xml")//country[language[contains(.,../@name)]]/data(@name)

Q10  (1/1 point)
Return all languages spoken in a country whose name textually contains the language name. For instance, German is spoken in Germany, so return German. (Hint: Depending on your solution, may want to use data(.), which returns the text value of the "current element" within an XPath expression.) 

doc("countries.xml")//country/language[contains(../@name,.)]/data(.)

Q11  (1/1 point)
Return all languages whose name textually contains the name of a country in which the language is spoken. For instance, Icelandic is spoken in Iceland, so return Icelandic. (Hint: Depending on your solution, may want to use data(.), which returns the text value of the "current element" within an XPath expression.) 

doc("countries.xml")//country/language[contains(.,../@name)]/data(.)

Q12  (1/1 point)
Return the number of countries where Russian is spoken. 

doc("countries.xml")/countries/count(country[language="Russian"])

Q13  (1/1 point)
Return the names of all countries for which the data does not include any languages or cities, but the country has more than 10 million people. 

doc("countries.xml")//country[count(language)=0 and count(city)=0 and @population>10000000]/data(@name)

Q14  (1/1 point)
Return the name of the country with the highest population. (Hint: You may need to explicitly cast population numbers as integers with xs:int() to get the correct answer.) 

doc("countries.xml")//country[@population=max(doc("countries.xml")//country/@population)]/data(@name)

Q15  (1/1 point)
Return the name of the country that has the city with the highest population. (Hint: You may need to explicitly cast population numbers as integers with xs:int() to get the correct answer.) 

doc("countries.xml")//country[city[population=max(doc("countries.xml")//population)]]/data(@name)

Q16  (1/1 point)
Return the average number of languages spoken in countries where Russian is spoken. 

avg(doc("countries.xml")//country[language="Russian"]/count(language))

Q17  (1/1 point)
Return all country-language pairs where the language is spoken in the country and the name of the country textually contains the language name. Return each pair as a country element with language attribute

for $a in doc("countries.xml")//country
for $b in $a/language
where $b[contains(../@name,.)]
return <country language="{data($b)}">
{$a/data(@name)}
</country>

Q18  (1/1 point)
Return all countries that have at least one city with population greater than 7 million. For each one, return the country name along with the cities greater than 7 million,

for $a in doc("countries.xml")//country
where $a/city/population>7000000
return <country name="{$a/data(@name)}">
{for $b in $a/city
where xs:int($b/population)>7000000
return <big>{$b/name/data(.)}</big>}
</country>

Q19  (1/1 point)
Return all countries where at least one language is listed, but the total percentage for all listed languages is less than 90%. Return the country element with its name attribute and its language subelements, but no other attributes or subelements. 

for $a in doc("countries.xml")//country[language]
where sum($a/language/data(@percentage))<90
return <country name="{$a/data(@name)}">
{$a/language}
</country>

Q20  (1/1 point)
Return all countries where at least one language is listed, and every listed language is spoken by less than 20% of the population. Return the country element with its name attribute and its language subelements, but no other attributes or subelements. 

for $a in doc("countries.xml")//country[language]
where every $b in $a/language satisfies $b/data(@percentage)<20
return <country name="{$a/data(@name)}">
{$a/language}
</country>

Q21  (1/1 point)
Find all situations where one country's most popular language is another country's least popular, and both countries list more than one language. (Hint: You may need to explicitly cast percentages as floating-point numbers with xs:float() to get the correct answer.) Return the name of the language and the two countries

let $a := doc("countries.xml")//country[count(language)>1]
let $most := 
  for $b in $a
  for $c in $b/language
  where xs:float($c/data(@percentage))=xs:float(max($b/language/data(@percentage)))
  return $c
let $least :=
  for $b in $a
  for $c in $b/language
  where xs:float($c/data(@percentage))=xs:float(min($b/language/data(@percentage)))
  return $c
for $b in $most
for $c in $least
where data($b)=data($c)
return <LangPair language="{data($b)}">
<MostPopular>{$b/../data(@name)}</MostPopular>
<LeastPopular>{$c/../data(@name)}</LeastPopular>
</LangPair>

Q22  (1/1 point)
For each language spoken in one or more countries, create a "language" element with a "name" attribute and one "country" subelement for each country in which the language is spoken. The "country" subelements should have two attributes: the country "name", and "speakers" containing the number of speakers of that language (based on language percentage and the country's population). Order the result by language name, and enclose the entire list in a single "languages" element. 

let $lang := distinct-values(doc("countries.xml")//language)
return <languages>
{for $l in $lang
order by $l
return <language name="{data($l)}">
{for $coun in doc("countries.xml")//country
for $lsub in $coun/language
where data($l)=data($lsub)
return <country name="{$coun/data(@name)}" speakers="{xs:int(($coun/@population)*($lsub/(@percentage div 100)))}"/>}
</language>}
</languages>