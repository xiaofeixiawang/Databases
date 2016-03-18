Q1  (1/1 point)
Return the course number of the course that is cross-listed as "LING180". 

doc("courses.xml")//Course[contains(Description,"LING180")]/data(@Number)

Q2  (1/1 point)
Return course numbers of courses that have the same title as some other course. (Hint: You might want to use the "preceding" and "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.) 

doc("courses.xml")//Course[Title=preceding::Title or Title=following::Title]/data(@Number)

Q3  (1/1 point)
Return course numbers of courses taught by an instructor with first name "Daphne" or "Julie". 

doc("courses.xml")//Course[Instructors/(Lecturer|Professor)[First_Name="Daphne" or First_Name="Julie"]]/data(@Number)

Q4  (1/1 point)
Return the number (count) of courses that have no lecturers as instructors. 

doc("courses.xml")/count(//Course[Instructors/count(Lecturer)=0])

Q5  (1/1 point)
Return titles of courses taught by the chair of a department. For this question, you may assume that all professors have distinct last names. 

doc("courses.xml")//Department/Course[Instructors//Last_Name=
preceding-sibling::Chair//Last_Name]/Title

Q6  (1/1 point)
Return titles of courses that have both a lecturer and a professor as instructors. Return each title only once. 

doc("courses.xml")//Course[Instructors[count(Lecturer)>=1 and count(Professor)>=1]]/Title

Q7  (1/1 point)
Return titles of courses taught by a professor with the last name "Ng" but not by a professor with the last name "Thrun". 

doc("courses.xml")//Course[Instructors[Professor/Last_Name="Ng" and count(Professor[Last_Name="Thrun"])=0]]/Title

Q8  (1/1 point)
Return course numbers of courses that have a course taught by Eric Roberts as a prerequisite. 

doc("courses.xml")//Course[Prerequisites/Prereq=//Course[Instructors/(Lecturer|Professor)[
Last_Name="Roberts" and First_Name="Eric"]]/data(@Number)]/data(@Number)

Q9  (1/1 point)
Create a summary of CS classes: List all CS department courses in order of enrollment. For each course include only its Enrollment (as an attribute) and its Title (as a subelement). 

<Summary>
{for $a in doc("courses.xml")/Course_Catalog/Department[@Code="CS"]/Course
order by xs:int($a/@Enrollment)
return
        <Course>
        {$a/@Enrollment}
        {$a/Title}
        </Course>}
</Summary>

Q10  (1/1 point)
Return a "Professors" element that contains as subelements a listing of all professors in all departments, sorted by last name with each professor appearing once. The "Professor" subelements should have the same structure as in the original data. For this question, you may assume that all professors have distinct last names. Watch out -- the presence/absence of middle initials may require some special handling. (This problem is quite challenging; congratulations if you get it right.) 

let $professor := doc("courses.xml")//Professor
let $distinct_professor := $professor
    except ( for $t in $professor 
            where $t/Last_Name=$t/following::*/Last_Name
            return $t )
return <Professors>
{for $t in $distinct_professor
order by $t/Last_Name
return $t}
</Professors>

Q11  (1/1 point)
Expanding on the previous question, create an inverted course listing: Return an "Inverted_Course_Catalog" element that contains as subelements professors together with the courses they teach, sorted by last name. You may still assume that all professors have distinct last names. The "Professor" subelements should have the same structure as in the original data, with an additional single "Courses" subelement under Professor, containing a further "Course" subelement for each course number taught by that professor. Professors who do not teach any courses should have no Courses subelement at all. (This problem is very challenging; extra congratulations if you get it right.) 

let $catalog := doc('courses.xml'),$professors := $catalog//Professor,$courses := $catalog//Course
let $distinct_prof := (
      $professors except (
        for $p in $professors
          where ($p/Last_Name = $p/following::*/Last_Name and $p/First_Name = $p/following::*/First_Name)
          return $p
      )
    )
return <Inverted_Course_Catalog>
  {
    for $p in $distinct_prof
      order by $p/Last_Name
      return <Professor>
      { $p/* }
      {
        if ($courses//Professor = $p) 
        then (
          <Courses> {
            for $c in $courses
              where $c//Professor = $p
              return <Course> { $c/data(@Number) } </Course>
          }
          </Courses>
        )
        else (
             )
      }
      </Professor>
  }
  </Inverted_Course_Catalog>