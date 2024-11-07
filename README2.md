-- mock data
-- Insert into Address table
INSERT INTO Address (Street, City, PostalCode) VALUES 
('123 Main St', 'Metropolis', '12345'),
('456 Elm St', 'Gotham', '67890'),
('789 Oak St', 'Star City', '10112'),
('321 Maple St', 'Metropolis', '54321'),
('654 Pine St', 'Gotham', '98765'),
('987 Birch St', 'Star City', '56789'),
('789 Willow St', 'Metropolis', '34567'),
('321 Cedar St', 'Gotham', '45678'),
('654 Chestnut St', 'Star City', '67890'),
('123 Spruce St', 'Metropolis', '23456'),
('456 Poplar St', 'Gotham', '12345');

-- Insert into Contact table
INSERT INTO Contact (Phone, Email) VALUES 
('555-1234', 'alice.smith@mail.com'),
('555-5678', 'bob.jones@mail.com'),
('555-8765', 'carol.davis@mail.com'),
('555-4321', 'john.doe@mail.com'),
('555-8765', 'emily.white@mail.com'),
('555-2345', 'robert.brown@mail.com'),
('555-1212', 'michael.johnson@mail.com'),
('555-1313', 'sarah.williams@mail.com'),
('555-1616', 'david.lee@mail.com'),
('555-1818', 'emily.brown@mail.com'),
('555-2020', 'jessica.taylor@mail.com');

-- Insert into Admin table with Contact reference
INSERT INTO Admin (Firstname, Lastname, AddressID, ContactID) VALUES 
('AdminFirst', 'AdminLast', 
(SELECT AddressID FROM Address WHERE Street = '123 Main St'), 
(SELECT ContactID FROM Contact WHERE Email = 'alice.smith@mail.com'));

-- Insert into Staff table with Contact reference
INSERT INTO Staff (Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('Alice', 'Smith', '1985-07-15', 
(SELECT AddressID FROM Address WHERE Street = '123 Main St'), 
(SELECT ContactID FROM Contact WHERE Email = 'alice.smith@mail.com')),

('Bob', 'Jones', '1979-11-30', 
(SELECT AddressID FROM Address WHERE Street = '456 Elm St'), 
(SELECT ContactID FROM Contact WHERE Email = 'bob.jones@mail.com')),

('Carol', 'Davis', '1990-03-20', 
(SELECT AddressID FROM Address WHERE Street = '789 Oak St'), 
(SELECT ContactID FROM Contact WHERE Email = 'carol.davis@mail.com'));

-- Insert into Doctor table with Contact reference
INSERT INTO Doctor (Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('Dr. John', 'Doe', '1975-02-18', 
(SELECT AddressID FROM Address WHERE Street = '321 Maple St'), 
(SELECT ContactID FROM Contact WHERE Email = 'john.doe@mail.com')),

('Dr. Emily', 'White', '1980-06-10', 
(SELECT AddressID FROM Address WHERE Street = '654 Pine St'), 
(SELECT ContactID FROM Contact WHERE Email = 'emily.white@mail.com')),

('Dr. Robert', 'Brown', '1983-12-25', 
(SELECT AddressID FROM Address WHERE Street = '987 Birch St'), 
(SELECT ContactID FROM Contact WHERE Email = 'robert.brown@mail.com'));

-- Insert into Patient table with Contact reference
INSERT INTO Patient (Firstname, Lastname, DOB, Sex, AddressID, ContactID, EmergencyContact) VALUES 
('Michael', 'Johnson', '1995-04-22', 'Male', 
(SELECT AddressID FROM Address WHERE Street = '789 Willow St'), 
(SELECT ContactID FROM Contact WHERE Email = 'michael.johnson@mail.com'), '555-1414'),

('Sarah', 'Williams', '1998-07-30', 'Female', 
(SELECT AddressID FROM Address WHERE Street = '321 Cedar St'), 
(SELECT ContactID FROM Contact WHERE Email = 'sarah.williams@mail.com'), '555-1515'),

('David', 'Lee', '2000-11-05', 'Male', 
(SELECT AddressID FROM Address WHERE Street = '654 Chestnut St'), 
(SELECT ContactID FROM Contact WHERE Email = 'david.lee@mail.com'), '555-1717'),

('Emily', 'Brown', '1992-02-18', 'Female', 
(SELECT AddressID FROM Address WHERE Street = '123 Spruce St'), 
(SELECT ContactID FROM Contact WHERE Email = 'emily.brown@mail.com'), '555-1919'),

('Jessica', 'Taylor', '1996-10-15', 'Female', 
(SELECT AddressID FROM Address WHERE Street = '456 Poplar St'), 
(SELECT ContactID FROM Contact WHERE Email = 'jessica.taylor@mail.com'), '555-2121');

-- Insert into UserAccount table
INSERT INTO UserAccount (UserType, Username, Password, ProfilePicture) VALUES 
('Admin', 'admin1', 'password123', NULL),
('Staff', 'alice_smith', 'staffpass1', NULL),
('Staff', 'bob_jones', 'staffpass2', NULL),
('Staff', 'carol_davis', 'staffpass3', NULL),
('Doctor', 'dr_john_doe', 'docpass1', NULL),
('Doctor', 'dr_emily_white', 'docpass2', NULL),
('Doctor', 'dr_robert_brown', 'docpass3', NULL),
('Patient', 'michael_j', 'patientpass1', NULL),
('Patient', 'sarah_w', 'patientpass2', NULL),
('Patient', 'david_l', 'patientpass3', NULL),
('Patient', 'emily_b', 'patientpass4', NULL),
('Patient', 'jessica_t', 'patientpass5', NULL);

-- Insert into AssignedTest table using subqueries based on names
INSERT INTO AssignedTest (PatientID, DoctorID, DateAssigned, TestType) VALUES
((SELECT PatientID FROM Patient WHERE Firstname = 'Michael' AND Lastname = 'Johnson'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-01', 'X-Ray'),

((SELECT PatientID FROM Patient WHERE Firstname = 'David' AND Lastname = 'Lee'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Emily' AND Lastname = 'White'), '2024-10-01', 'ECG'),

((SELECT PatientID FROM Patient WHERE Firstname = 'David' AND Lastname = 'Lee'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Emily' AND Lastname = 'White'), '2024-10-01', 'Ultrasound'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Sarah' AND Lastname = 'Williams'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-02', 'CTScan'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Emily' AND Lastname = 'Brown'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-03', 'X-Ray'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Emily' AND Lastname = 'Brown'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-03', 'UrineTest'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Michael' AND Lastname = 'Johnson'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Robert' AND Lastname = 'Brown'), '2024-10-04', 'CTScan');

-- Insert into AssignedBloodTest table using subqueries based on names
INSERT INTO AssignedBloodTest (PatientID, DoctorID, DateAssigned, BloodTestType) VALUES
((SELECT PatientID FROM Patient WHERE Firstname = 'Jessica' AND Lastname = 'Taylor'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-01', 'Coagulation'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Jessica' AND Lastname = 'Taylor'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-01', 'LiverFunction'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Michael' AND Lastname = 'Johnson'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Emily' AND Lastname = 'White'), '2024-10-02', 'RoutineHematology'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Michael' AND Lastname = 'Johnson'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Emily' AND Lastname = 'White'), '2024-10-02', 'TumorMarker'),

((SELECT PatientID FROM Patient WHERE Firstname = 'David' AND Lastname = 'Lee'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-02', 'PancreasFunction'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Jessica' AND Lastname = 'Taylor'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Robert' AND Lastname = 'Brown'), '2024-10-02', 'RenalFunction'),

((SELECT PatientID FROM Patient WHERE Firstname = 'David' AND Lastname = 'Lee'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Emily' AND Lastname = 'White'), '2024-10-03', 'RoutineHematology'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Sarah' AND Lastname = 'Williams'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. John' AND Lastname = 'Doe'), '2024-10-04', 'PancreasFunction'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Emily' AND Lastname = 'Brown'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Robert' AND Lastname = 'Brown'), '2024-10-05', 'RenalFunction'),

((SELECT PatientID FROM Patient WHERE Firstname = 'Emily' AND Lastname = 'Brown'), 
 (SELECT DoctorID FROM Doctor WHERE Firstname = 'Dr. Robert' AND Lastname = 'Brown'), '2024-10-05', 'Endocrinology');
