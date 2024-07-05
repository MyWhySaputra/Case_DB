### 1. List of Students with Their Classes and Teachers

```
SELECT s.name AS student_name, c.name AS class_name, t.name AS teacher_name
FROM students s
JOIN classes c ON s.class_id = c.id
JOIN teachers t ON c.teacher_id = t.id;
```

### 2. List of Classes Taught by the Same Teacher

```
SELECT c.name AS class_name, t.name AS teacher_name
FROM classes c
JOIN teachers t ON c.teacher_id = t.id;
```

### 3. Create View for Students, Classes, and Teachers

```
CREATE VIEW student_class_teacher AS
SELECT s.name AS student_name, c.name AS class_name, t.name AS teacher_name
FROM students s
JOIN classes c ON s.class_id = c.id
JOIN teachers t ON c.teacher_id = t.id;
```

### 4. Stored Procedure to Retrieve Students, Classes, and Teachers

```
DELIMITER //

CREATE PROCEDURE GetStudentClassTeacher()
BEGIN
    SELECT s.name AS student_name, c.name AS class_name, t.name AS teacher_name
    FROM students s
    JOIN classes c ON s.class_id = c.id
    JOIN teachers t ON c.teacher_id = t.id;
END //

DELIMITER ;
```

### 5. Stored Procedure for Inserting Students with Duplicate Check

```
DELIMITER //

CREATE PROCEDURE InsertStudentWithCheck(
    IN p_name VARCHAR(100),
    IN p_age INT,
    IN p_class_id INT
)
BEGIN
    DECLARE duplicate_key CONDITION FOR SQLSTATE '23000';
    
    DECLARE CONTINUE HANDLER FOR duplicate_key
    BEGIN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Error: Duplicate entry for student in the same class';
    END;

    INSERT INTO students (name, age, class_id)
    VALUES (p_name, p_age, p_class_id);
END //

DELIMITER ;
```
