# Design DB model for Guvi Zen class****
##  -- Users Table
> CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

## -- Class Table
CREATE TABLE class (
    class_id INT AUTO_INCREMENT PRIMARY KEY,
    class_name VARCHAR(100) NOT NULL,
    class_description TEXT
);

## -- Students Table
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    class_id INT NOT NULL,
    enrollment_date DATE NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Task Table
CREATE TABLE task (
    task_id INT AUTO_INCREMENT PRIMARY KEY,
    task_name VARCHAR(100) NOT NULL,
    task_description TEXT,
    assigned_date DATE NOT NULL,
    due_date DATE NOT NULL,
    class_id INT NOT NULL,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Mentor Table
CREATE TABLE mentor (
    mentor_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    class_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Topics Table
CREATE TABLE topics (
    topic_id INT AUTO_INCREMENT PRIMARY KEY,
    topic_name VARCHAR(100) NOT NULL,
    topic_description TEXT,
    class_id INT NOT NULL,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Assessments Table
CREATE TABLE assessments (
    assessment_id INT AUTO_INCREMENT PRIMARY KEY,
    assessment_name VARCHAR(100) NOT NULL,
    assessment_description TEXT,
    class_id INT NOT NULL,
    date DATE NOT NULL,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Attendance Table
CREATE TABLE attendance (
    attendance_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    class_id INT NOT NULL,
    attendance_date DATE NOT NULL,
    status VARCHAR(10) CHECK (status IN ('Present', 'Absent', 'Late')),
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (class_id) REFERENCES class(class_id) ON DELETE CASCADE
);

## -- Queries Table
CREATE TABLE queries (
    query_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    mentor_id INT NOT NULL,
    query_text TEXT NOT NULL,
    query_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (mentor_id) REFERENCES mentor(mentor_id) ON DELETE CASCADE
);

## -- Interview Table
CREATE TABLE interview (
    interview_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    mentor_id INT NOT NULL,
    interview_date DATE NOT NULL,
    feedback TEXT,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (mentor_id) REFERENCES mentor(mentor_id) ON DELETE CASCADE
);

## -- Student Details Table
CREATE TABLE student_details (
    detail_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE,
    address TEXT,
    phone_number VARCHAR(20),
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE
);

## -- Insert Data into Users Table
INSERT INTO users (username, password, email) VALUES 
('john_doe', 'password123', 'john@example.com'),
('jane_smith', 'securepass', 'jane@example.com'),
('mentor_mike', 'mentorpass', 'mike@example.com');

## -- Insert Data into Class Table
INSERT INTO class (class_name, class_description) VALUES 
('Full-Stack Development', 'A comprehensive course covering both front-end and back-end development');

## -- Insert Data into Students Table
INSERT INTO students (user_id, class_id, enrollment_date) VALUES 
(1, 1, '2024-01-15'),
(2, 1, '2024-01-15');

## -- Insert Data into Task Table
INSERT INTO task (task_name, task_description, assigned_date, due_date, class_id) VALUES 
('HTML/CSS Task', 'Create a responsive web page using HTML and CSS', '2024-01-16', '2024-01-23', 1),
('JavaScript Task', 'Implement interactive features using JavaScript', '2024-01-24', '2024-01-31', 1),
('Backend Task', 'Build a REST API using Node.js and Express', '2024-02-01', '2024-02-08', 1);
## 
-- Insert Data into Mentor Table
INSERT INTO mentor (user_id, class_id) VALUES 
(3, 1);

## -- Insert Data into Topics Table
INSERT INTO topics (topic_name, topic_description, class_id) VALUES 
('HTML/CSS', 'Introduction to HTML and CSS for web development', 1),
('JavaScript', 'Introduction to JavaScript programming', 1),
('Node.js and Express', 'Building back-end services with Node.js and Express', 1),
('Database', 'Working with databases using SQL and NoSQL', 1),
('React', 'Building dynamic user interfaces with React', 1);

## -- Insert Data into Assessments Table
INSERT INTO assessments (assessment_name, assessment_description, class_id, date) VALUES 
('HTML/CSS Assessment', 'Assessment on HTML and CSS', 1, '2024-02-10'),
('JavaScript Assessment', 'Assessment on JavaScript programming', 1, '2024-02-20'),
('Backend Assessment', 'Assessment on building REST APIs', 1, '2024-02-28');

## -- Insert Data into Attendance Table
INSERT INTO attendance (student_id, class_id, attendance_date, status) VALUES 
(1, 1, '2024-01-16', 'Present'),
(2, 1, '2024-01-16', 'Present'),
(1, 1, '2024-01-17', 'Absent'),
(2, 1, '2024-01-17', 'Present');

## -- Insert Data into Queries Table
INSERT INTO queries (student_id, mentor_id, query_text) VALUES 
(1, 1, 'Can you explain how to make a webpage responsive?'),
(2, 1, 'I need help with understanding JavaScript closures.');

## -- Insert Data into Interview Table
INSERT INTO interview (student_id, mentor_id, interview_date, feedback) VALUES 
(1, 1, '2024-03-01', 'Good progress in understanding HTML and CSS. Needs more practice on JavaScript.'),
(2, 1, '2024-03-02', 'Excellent understanding of JavaScript concepts. Keep up the good work.');

## -- Insert Data into Student Details Table
INSERT INTO student_details (student_id, first_name, last_name, date_of_birth, address, phone_number) VALUES 
(1, 'John', 'Doe', '2000-01-01', '123 Main St, Cityville', '123-456-7890'),
(2, 'Jane', 'Smith', '1999-02-02', '456 Elm St, Townsville', '098-765-4321');
