<!-- QUERY CON SELECT -->

1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`)= 1990;

2. Selezionare tutti i corsi che valgono più di 10 credici (479)

SELECT * 
FROM `courses` 
WHERE `cfu`>10;

3. Selezionare tutti gli studenti che hanno più di 30 anni 

SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`)> 1992;


4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

SELECT * FROM `courses` 
WHERE `YEAR` = "1" AND `period` = "I semestre";


5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14)  20/06/2020 (21)

SELECT * FROM `exams`
WHERE `hour`> "14:00:00" 
AND `date`= '2020-06-20'

6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT * FROM `degrees` 
WHERE `level`= 'magistrale';


7. Da quanti dipartimenti è composta l'università? (12)

SELECT COUNT(*) 
AS `num_departmens` 
FROM `departments`;


8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT * 
FROM `teachers` 
WHERE `phone` IS NULL;





<!-- QUERY CON GROUP BY -->
1. Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(*) AS 'num_students'
YEAR (`enrolment_date`)
FROM `students` 
GROUP BY YEAR (`enrolment_date`);


2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT COUNT(*) `office_address` 
FROM `teachers` 
GROUP BY `office_address`;


3. Calcolare la media dei voti di ogni appello d'esame

SELECT AVG (`vote), `exam_id`
FROM `exam_student` 
GROUP BY `exam_id`;


4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT COUNT(*),department_id
FROM `degrees`
GROUP BY department_id;



<!-- QUERY CON JOIN -->
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.`name` AS 'name', `students`.`surname` AS 'surname', `students`.`registration_number` AS 'student_id', `students`.`email` AS 'email', `degrees`.`name` 
AS 'deegrees_name'
FROM `degrees` 
JOIN `students`
ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = 'Corso di laurea in Economia';



2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze

SELECT `departments`.`name` AS 'departments', `degrees`.`name` AS 'deegrees_name'
FROM `departments` 
JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` LIKE '%Neuroscienze%';


3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `teachers`.`name`, `teachers`.`surname`, `courses`.`name`
FROM `teachers`
JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses`
ON `courses`.`id` = `course_teacher`.`course_id`
WHERE `teachers`.`name` = 'Fulvio'
AND `teachers`.`surname` = 'Amato';

<!-- *
SELECT `teachers`.`id` AS "insegnanti",
`teachers`.`name`, `teachers`.`surname`, `courses`.`name`
FROM `teachers`
JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses`
ON `courses`.`id` = `course_teacher`.`course_id`
WHERE `teachers`.`name` = 'Fulvio'
AND `teachers`.`surname` = 'Amato';
WHERE `teachers`.`id` = '44' -->

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT `students`.`id`, `students`.`name`AS "name_student", `students`.`surname` AS "name_surname", `degrees`.`name`, `departments`.`name`
FROM `students`
JOIN `degrees`
ON `degrees`.`id` = `students`.`degree_id`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname` ASC, `students`.`name` ASC;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT `degrees`.`name`AS "name_degrees", `courses`.`name`AS "name_courses", `teachers`.`name` AS "teacher_course", `teachers`.`surname` AS "teacher_course" ;
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;


6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

SELECT `teachers`.`name`, `teachers`.`surname`, `departments`.`name`
FROM `teachers`
JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees`
ON `degrees`.`id` = `courses`.`degree_id`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di matematica';  

7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami

