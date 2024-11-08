-- Insert into Address table with manual IDs
INSERT INTO Address (AddressID, Street, City, PostalCode) VALUES 
('ADR_1', '123 Main St', 'Metropolis', '12345'),
('ADR_2', '456 Elm St', 'Gotham', '67890'),
('ADR_3', '789 Oak St', 'Star City', '10112'),
('ADR_4', '321 Maple St', 'Metropolis', '54321'),
('ADR_5', '654 Pine St', 'Gotham', '98765'),
('ADR_6', '987 Birch St', 'Star City', '56789'),
('ADR_7', '789 Willow St', 'Metropolis', '34567'),
('ADR_8', '321 Cedar St', 'Gotham', '45678'),
('ADR_9', '654 Chestnut St', 'Star City', '67890'),
('ADR_10', '123 Spruce St', 'Metropolis', '23456'),
('ADR_11', '456 Poplar St', 'Gotham', '12345');

-- Insert into Contact table with manual IDs
INSERT INTO Contact (ContactID, Phone, Email) VALUES 
('CNT_1', '555-1234', 'alice.smith@mail.com'),
('CNT_2', '555-5678', 'bob.jones@mail.com'),
('CNT_3', '555-8765', 'carol.davis@mail.com'),
('CNT_4', '555-4321', 'john.doe@mail.com'),
('CNT_5', '555-8765', 'emily.white@mail.com'),
('CNT_6', '555-2345', 'robert.brown@mail.com'),
('CNT_7', '555-1212', 'michael.johnson@mail.com'),
('CNT_8', '555-1313', 'sarah.williams@mail.com'),
('CNT_9', '555-1616', 'david.lee@mail.com'),
('CNT_10', '555-1818', 'emily.brown@mail.com'),
('CNT_11', '555-2020', 'jessica.taylor@mail.com');

-- Insert into Admin table with manual IDs
INSERT INTO Admin (AdminID, Firstname, Lastname, AddressID, ContactID) VALUES 
('ADM_1', 'AdminFirst', 'AdminLast', 'ADR_1', 'CNT_1');

-- Insert into Staff table with manual IDs
INSERT INTO Staff (StaffID, Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('STF_1', 'Alice', 'Smith', '1985-07-15', 'ADR_1', 'CNT_1'),
('STF_2', 'Bob', 'Jones', '1979-11-30', 'ADR_2', 'CNT_2'),
('STF_3', 'Carol', 'Davis', '1990-03-20', 'ADR_3', 'CNT_3');

-- Insert into Doctor table with manual IDs
INSERT INTO Doctor (DoctorID, Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('DOC_1', 'Dr. John', 'Doe', '1975-02-18', 'ADR_4', 'CNT_4'),
('DOC_2', 'Dr. Emily', 'White', '1980-06-10', 'ADR_5', 'CNT_5'),
('DOC_3', 'Dr. Robert', 'Brown', '1983-12-25', 'ADR_6', 'CNT_6');

-- Insert into Patient table with manual IDs
INSERT INTO Patient (PatientID, Firstname, Lastname, DOB, Sex, AddressID, ContactID, EmergencyContact) VALUES 
('PAT_1', 'Michael', 'Johnson', '1995-04-22', 'Male', 'ADR_7', 'CNT_7', '555-1414'),
('PAT_2', 'Sarah', 'Williams', '1998-07-30', 'Female', 'ADR_8', 'CNT_8', '555-1515'),
('PAT_3', 'David', 'Lee', '2000-11-05', 'Male', 'ADR_9', 'CNT_9', '555-1717'),
('PAT_4', 'Emily', 'Brown', '1992-02-18', 'Female', 'ADR_10', 'CNT_10', '555-1919'),
('PAT_5', 'Jessica', 'Taylor', '1996-10-15', 'Female', 'ADR_11', 'CNT_11', '555-2121');

-- Insert into UserAccount table with manual IDs
INSERT INTO UserAccount (UserAccountID, UserType, Username, Password, ProfilePicture) VALUES 
('USR_1', 'Admin', 'admin1', 'password123', NULL),
('USR_2', 'Staff', 'alice_smith', 'staffpass1', NULL),
('USR_3', 'Staff', 'bob_jones', 'staffpass2', NULL),
('USR_4', 'Staff', 'carol_davis', 'staffpass3', NULL),
('USR_5', 'Doctor', 'dr_john_doe', 'docpass1', NULL),
('USR_6', 'Doctor', 'dr_emily_white', 'docpass2', NULL),
('USR_7', 'Doctor', 'dr_robert_brown', 'docpass3', NULL),
('USR_8', 'Patient', 'michael_j', 'patientpass1', NULL),
('USR_9', 'Patient', 'sarah_w', 'patientpass2', NULL),
('USR_10', 'Patient', 'david_l', 'patientpass3', NULL),
('USR_11', 'Patient', 'emily_b', 'patientpass4', NULL),
('USR_12', 'Patient', 'jessica_t', 'patientpass5', NULL);

-- Insert into AssignedTest table with manual IDs
INSERT INTO AssignedTest (AssignedTestID, PatientID, DoctorID, DateAssigned, TestType) VALUES
('AST_1', 'PAT_1', 'DOC_1', '2024-10-01', 'X-Ray'),
('AST_2', 'PAT_3', 'DOC_2', '2024-10-01', 'ECG'),
('AST_3', 'PAT_3', 'DOC_2', '2024-10-01', 'Ultrasound'),
('AST_4', 'PAT_2', 'DOC_1', '2024-10-02', 'CTScan'),
('AST_5', 'PAT_4', 'DOC_1', '2024-10-03', 'X-Ray'),
('AST_6', 'PAT_4', 'DOC_1', '2024-10-03', 'UrineTest'),
('AST_7', 'PAT_1', 'DOC_3', '2024-10-04', 'CTScan');

-- Insert into AssignedBloodTest table with IDs
INSERT INTO AssignedBloodTest (AssignedBloodTestID, PatientID, DoctorID, DateAssigned, BloodTestType) VALUES
('ABT_1', 'PAT_5', 'DOC_1', '2024-10-01', 'Coagulation'),
('ABT_2', 'PAT_5', 'DOC_1', '2024-10-01', 'LiverFunction'),
('ABT_3', 'PAT_1', 'DOC_2', '2024-10-02', 'RoutineHematology'),
('ABT_4', 'PAT_1', 'DOC_2', '2024-10-02', 'TumorMarker'),
('ABT_5', 'PAT_3', 'DOC_1', '2024-10-02', 'PancreasFunction'),
('ABT_6', 'PAT_5', 'DOC_3', '2024-10-02', 'RenalFunction'),
('ABT_7', 'PAT_3', 'DOC_2', '2024-10-03', 'RoutineHematology'),
('ABT_8', 'PAT_2', 'DOC_1', '2024-10-04', 'PancreasFunction'),
('ABT_9', 'PAT_4', 'DOC_3', '2024-10-05', 'RenalFunction'),
('ABT_10', 'PAT_4', 'DOC_3', '2024-10-05', 'Endocrinology');
