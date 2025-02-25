### Summary and Comparison of Normalization Forms: 1NF, 2NF, and 3NF

#### First Normal Form (1NF)
**Criteria:**
- All columns must contain atomic (indivisible) values.
- Each column must contain values of a single type.
  
**Example (Non-1NF Table):**

| EmployeeID | EmployeeName | PhoneNumbers           |
|------------|--------------|------------------------|
| 1          | John Doe     | 555-1234, 555-5678     |
| 2          | Jane Smith   | 555-8765               |
| 3          | Alice Brown  | 555-4321, 555-0000, 555-1111 |

**Converted to 1NF:**

| EmployeeID | EmployeeName | PhoneNumber |
|------------|--------------|-------------|
| 1          | John Doe     | 555-1234    |
| 1          | John Doe     | 555-5678    |
| 2          | Jane Smith   | 555-8765    |
| 3          | Alice Brown  | 555-4321    |
| 3          | Alice Brown  | 555-0000    |
| 3          | Alice Brown  | 555-1111    |

#### Second Normal Form (2NF)
**Criteria:**
- The table must be in 1NF.
- All non-key columns must be fully dependent on the entire primary key (no partial dependencies).

**Example (1NF Table with Partial Dependencies):**

| StudentID | StudentName | CourseName      | Instructor     | InstructorOffice |
|-----------|-------------|-----------------|----------------|------------------|
| 1         | Alice       | Database Design | Dr. Smith      | Room 101         |
| 1         | Alice       | Algorithms      | Dr. Johnson    | Room 202         |
| 2         | Bob         | Algorithms      | Dr. Johnson    | Room 202         |
| 3         | Charlie     | Database Design | Dr. Smith      | Room 101         |

**Converted to 2NF:**

**Students Table**

| StudentID | StudentName |
|-----------|-------------|
| 1         | Alice       |
| 2         | Bob         |
| 3         | Charlie     |

**Courses Table**

| CourseName      | Instructor  | InstructorOffice |
|-----------------|-------------|------------------|
| Database Design | Dr. Smith   | Room 101         |
| Algorithms      | Dr. Johnson | Room 202         |

**StudentCourses Table**

| StudentID | CourseName      |
|-----------|-----------------|
| 1         | Database Design |
| 1         | Algorithms      |
| 2         | Algorithms      |
| 3         | Database Design |

#### Third Normal Form (3NF)
**Criteria:**
- The table must be in 2NF.
- All non-key columns must be directly dependent on the primary key (no transitive dependencies).

**Example (2NF Table with Transitive Dependencies):**

**Courses Table**

| CourseName      | Instructor  | InstructorOffice |
|-----------------|-------------|------------------|
| Database Design | Dr. Smith   | Room 101         |
| Algorithms      | Dr. Johnson | Room 202         |

**Transitive Dependency:**
- `InstructorOffice` depends on `Instructor`, which depends on `CourseName`.

**Converted to 3NF:**

**Courses Table**

| CourseName      | InstructorID |
|-----------------|--------------|
| Database Design | 1            |
| Algorithms      | 2            |

**Instructors Table**

| InstructorID | Instructor  | InstructorOffice |
|--------------|-------------|------------------|
| 1            | Dr. Smith   | Room 101         |
| 2            | Dr. Johnson | Room 202         |

### Comparison Summary

| Normal Form  | Criteria                                                                                                     | Example Issue Resolved                                   |
|--------------|--------------------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| **1NF**      | - Columns contain atomic values.<br>- Columns contain values of a single type.                              | Converts multi-valued columns into atomic values.       |
| **2NF**      | - Table is in 1NF.<br>- Non-key columns are fully dependent on the entire primary key (no partial dependency). | Splits tables to eliminate partial dependencies.        |
| **3NF**      | - Table is in 2NF.<br>- Non-key columns are directly dependent on the primary key (no transitive dependency). | Splits tables to eliminate transitive dependencies.     |

This comparison highlights how each normal form builds upon the previous one to improve database structure, reduce redundancy, and ensure data integrity.