Q1  (1/1 point)
Return all Title elements (of both departments and courses). 

doc("courses.xml")//Title

Q2  (1/1 point)
Return last names of all department chairs. 

doc("courses.xml")//Chair/(Lecturer|Professor)/Last_Name

Q3  (1/1 point)
Return titles of courses with enrollment greater than 500. 

doc("courses.xml")//Course[@Enrollment>500]/Title

Q4  (1/1 point)
Return titles of departments that have some course that takes "CS106B" as a prerequisite. 

doc("courses.xml")//Department[Course/Prerequisites/Prereq="CS106B"]/Title

Q5  (1/1 point)
Return last names of all professors or lecturers who use a middle initial. Don't worry about eliminating duplicates. 

doc("courses.xml")//(Professor|Lecturer)[Middle_Initial]/Last_Name

Q6  (1/1 point)
Return the count of courses that have a cross-listed course (i.e., that have "Cross-listed" in their description). 

doc("courses.xml")/count(//Course[contains(Description,"Cross-listed")])

Q7  (1/1 point)
Return the average enrollment of all courses in the CS department. 

doc("courses.xml")//Department[@Code="CS"]/avg(Course/@Enrollment)

Q8  (1/1 point)
Return last names of instructors teaching at least one course that has "system" in its description and enrollment greater than 100. 

doc("courses.xml")//Course[@Enrollment>100 and contains(Description,"system")]/Instructors//Last_Name

Q9  (1/1 point)
Return the title of the course with the largest enrollment. 

doc("courses.xml")//Course[@Enrollment= max(//Course/@Enrollment)]/Title