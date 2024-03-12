# QUERY CON GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT COUNT(*) AS `numero_iscritti`, YEAR(`enrolment_date`) AS `anno` FROM `students`
GROUP BY YEAR(`enrolment_date`);
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT COUNT(*) AS `numero_insegnanti`, `office_address` AS `indirizzo_ufficio` FROM `teachers`
GROUP BY `office_address`;
```

3. Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT `exam_id` AS `appello_esame`, AVG(`vote`) AS `media_voti` FROM `exam_student`
GROUP BY `exam_id`;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT `department_id` AS `dipartimento`, COUNT(`name`) AS `numero_corsi` FROM `degrees`
GROUP BY `department_id`;
```



# QUERY CON JOIN 

1.  Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT 
    `students`.`name` AS `nome_studente`,
    `students`.`surname`AS `cognome_studente`,
    `degrees`.`name` AS `corso_di_laurea`
FROM `students`
INNER JOIN `degrees`
ON `degrees`.`department_id` = `students`.`degree_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

2.  Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

```sql
SELECT 
    `level` AS `corso_magistrale`,
    `degrees`.`name` AS `nome_corso`,
    `departments`.`name` AS `nome_dipartimento`
FROM `degrees`
INNER JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `level` = 'magistrale'  AND `departments`.`name` = 'Dipartimento di Neuroscienze';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT 
    `teachers`.`name` AS `nome_insegnante`, 
    `teachers`.`surname` AS `cognome_insegnante`,
    `courses`.`name` AS `nome_corso`
FROM `teachers`
INNER JOIN `courses`
ON `courses`.`id`
WHERE `teachers`.`id` = '44';
```

4.  Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

```sql
SELECT 
    `students`.`name`,
    `students`.`surname`,
    `degrees`.`name` AS `nome_corso`,
    `departments`.`name` AS `nome_dipartimento`
FROM `students`
INNER JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
INNER JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`

ORDER BY `students`.`name`, `students`.`surname`ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT
	`degrees`.`name` AS `corso_di_laurea`,
	`courses`.`name`AS `nome_corso`,
	`teachers`.`name` AS `nome_insegnante`,
	`teachers`.`surname` as `cognome_insegnante`
FROM `degrees`
INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `teachers`
ON `teachers`.`id` = `courses`.`id`;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

```sql
SELECT DISTINCT
	`teachers`.`name` AS `nome_insegnante`,
	`teachers`.`surname` AS `cognome_insegnante`,
    `departments`.`name` AS `nome_dipartimento`
FROM `teachers`
INNER JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
INNER JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
INNER JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`
INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18