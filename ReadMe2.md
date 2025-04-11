Utilizzando lo stesso database di ieri, eseguite le query in allegato. Caricate un secondo file nella stessa repo di ieri (db-university) con le query di oggi.
 JOIN
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
 GROUP BY
1. Contare quanti iscritti ci sono stati ogni anno
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento


1. SELECT students.name, students.surname, degrees.name 
FROM students
JOIN degrees ON degrees.id = students.degree_id
WHERE degrees.name = 'Corso di Laurea in Economia'
2.SELECT *
FROM degrees
JOIN departments ON departments.id = degrees.department_id
WHERE degrees.level = 'magistrale' AND departments.name = 'Dipartimento di Neuroscienze'
3. SELECT courses.name, teachers.name, teachers.surname
FROM course_teacher
JOIN teachers ON teachers.id = course_teacher.teacher_id
JOIN courses ON courses.id = course_teacher.course_id
WHERE teachers.id = 44
4.SELECT students.name, students.surname, departments.name, degrees.name
FROM students
JOIN degrees ON degrees.id = students.degree_id
JOIN departments ON departments.id = degrees.department_id
ORDER BY students.surname ASC, students.name ASC
5. SELECT teachers.name, teachers.surname, degrees.name, courses.name
FROM degrees
JOIN courses ON degrees.id = courses.degree_id
JOIN course_teacher ON course_teacher.course_id = courses.id
JOIN teachers ON teachers.id = course_teacher.teacher_id 
6. SELECT teachers.name, teachers.surname, departments.name
FROM teachers
JOIN course_teacher ON teachers.id = course_teacher.teacher_id
JOIN courses ON course_teacher.course_id = courses.id
JOIN degrees ON degrees.id = courses.degree_id
JOIN departments ON departments.id = degrees.department_id
WHERE departments.name = 'Dipartimento di Matematica'(?)
 

##GROUP BY
1.SELECT COUNT(ID) AS total_subscriptions, YEAR(enrolment_date)
FROM students
GROUP BY YEAR(enrolment_date)

2. SELECT COUNT(ID), teachers.office_address
FROM teachers
GROUP BY office_address
3.SELECT AVG(vote), exams.id
FROM exams
JOIN exam_student ON exam_student.exam_id = exams.id
JOIN students ON exam_student.student_id = students.id
GROUP BY exams.id
4.SELECT COUNT(degrees.id) AS 'corsi di laurea', departments.name
FROM degrees
JOIN departments on departments.id = degrees.department_id
GROUP BY departments.id