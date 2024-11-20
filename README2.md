-- 1. Address Table Insert
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

-- 2. Contact Table Insert
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

-- 3. Admin Table Insert
INSERT INTO Admin (AdminID, Firstname, Lastname, AddressID, ContactID) VALUES 
('ADM_1', 'AdminFirst', 'AdminLast', 'ADR_1', 'CNT_1');

-- 4. Staff Table Insert
INSERT INTO Staff (StaffID, Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('STF_1', 'Alice', 'Smith', '1985-07-15', 'ADR_1', 'CNT_1'),
('STF_2', 'Bob', 'Jones', '1979-11-30', 'ADR_2', 'CNT_2'),
('STF_3', 'Carol', 'Davis', '1990-03-20', 'ADR_3', 'CNT_3');

-- 5. Doctor Table Insert
INSERT INTO Doctor (DoctorID, Firstname, Lastname, DOB, AddressID, ContactID) VALUES 
('DOC_1', 'Dr. John', 'Doe', '1975-02-18', 'ADR_4', 'CNT_4'),
('DOC_2', 'Dr. Emily', 'White', '1980-06-10', 'ADR_5', 'CNT_5'),
('DOC_3', 'Dr. Robert', 'Brown', '1983-12-25', 'ADR_6', 'CNT_6');

-- 6. Patient Table Insert
INSERT INTO Patient (PatientID, Firstname, Lastname, DOB, Sex, AddressID, ContactID, EmergencyContact) VALUES 
('PAT_1', 'Michael', 'Johnson', '1995-04-22', 'Male', 'ADR_7', 'CNT_7', '555-1414'),
('PAT_2', 'Sarah', 'Williams', '1998-07-30', 'Female', 'ADR_8', 'CNT_8', '555-1515'),
('PAT_3', 'David', 'Lee', '2000-11-05', 'Male', 'ADR_9', 'CNT_9', '555-1717'),
('PAT_4', 'Emily', 'Brown', '1992-02-18', 'Female', 'ADR_10', 'CNT_10', '555-1919'),
('PAT_5', 'Jessica', 'Taylor', '1996-10-15', 'Female', 'ADR_11', 'CNT_11', '555-2121');

-- 7. UserAccount Table Insert with Check Constraint
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

-- Add a check constraint to enforce valid user types
ALTER TABLE UserAccount
ADD CONSTRAINT chk_UserType CHECK (UserType IN ('Admin', 'Staff', 'Doctor', 'Patient'));

-- 8. AssignedTest Table Insert
INSERT INTO AssignedTest (AssignedTestID, PatientID, DoctorID, DateAssigned, TestType) VALUES
('AST_1', 'PAT_1', 'DOC_1', '2024-10-01', 'X-Ray'),
('AST_2', 'PAT_3', 'DOC_2', '2024-10-01', 'ECG'),
('AST_3', 'PAT_3', 'DOC_2', '2024-10-01', 'Ultrasound'),
('AST_4', 'PAT_2', 'DOC_1', '2024-10-02', 'CTScan'),
('AST_5', 'PAT_4', 'DOC_1', '2024-10-03', 'X-Ray'),
('AST_6', 'PAT_4', 'DOC_1', '2024-10-03', 'UrineTest'),
('AST_7', 'PAT_1', 'DOC_3', '2024-10-04', 'CTScan');

-- 9. AssignedBloodTest Table Insert
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

INSERT INTO TestResult (TestResultID, StaffID, PatientID, AssignedTestID, AssignedBloodTestID, DateUpdated, DoctorNote, Result) VALUES
('TR_1', 'STF_1', 'PAT_1', 'AST_1', NULL, '2024-10-10', NULL, NULL),
('TR_2', 'STF_2', 'PAT_4', 'AST_5', NULL, '2024-10-10', NULL, NULL),
('TR_3', 'STF_3', 'PAT_3', 'AST_2', NULL, '2024-10-10', NULL, NULL),
('TR_4', 'STF_3', 'PAT_3', 'AST_3', NULL, '2024-10-11', NULL, NULL),
('TR_5', 'STF_3', 'PAT_2', 'AST_4', NULL, '2024-10-12', NULL, NULL),
('TR_6', 'STF_3', 'PAT_1', 'AST_7', NULL, '2024-10-12', NULL, NULL),
('TR_7', 'STF_1', 'PAT_4', 'AST_6', NULL, '2024-10-12', NULL, 'Abnormal'),
('TR_8', 'STF_2', 'PAT_5', NULL, 'ABT_1', '2024-10-12', NULL, 'Normal'),
('TR_9', 'STF_2', 'PAT_5', NULL, 'ABT_2', '2024-10-12', NULL, 'Abnormal'),
('TR_10', 'STF_1', 'PAT_1', NULL, 'ABT_3', '2024-10-12', NULL, 'Normal'),
('TR_11', 'STF_1', 'PAT_3', NULL, 'ABT_7', '2024-10-12', NULL, 'Normal'),
('TR_12', 'STF_1', 'PAT_1', NULL, 'ABT_4', '2024-10-13', NULL, 'Abnormal'),
('TR_13', 'STF_1', 'PAT_3', NULL, 'ABT_5', '2024-10-14', NULL, 'Normal'),
('TR_14', 'STF_2', 'PAT_2', NULL, 'ABT_8', '2024-10-14', NULL, 'Normal'),
('TR_15', 'STF_2', 'PAT_5', NULL, 'ABT_6', '2024-10-14', NULL, 'Normal'),
('TR_16', 'STF_1', 'PAT_4', NULL, 'ABT_9', '2024-10-15', NULL, 'Normal'),
('TR_17', 'STF_3', 'PAT_4', NULL, 'ABT_10', '2024-10-15', NULL, 'Normal');

INSERT INTO ImagingResultDetails (TestResultID, Image) VALUES
('TR_1', NULL),  
('TR_2', NULL),  
('TR_3', NULL),  
('TR_4', NULL),  
('TR_5', NULL),  
('TR_6', NULL);  

-- XRayResult Table
INSERT INTO XRayResult (XRayResultID, TestResultID) VALUES
('XRR_1', 'TR_1'),
('XRR_2', 'TR_2');

-- UltrasoundResult Table
INSERT INTO UltrasoundResult (UltrasoundResultID, TestResultID) VALUES
('USR_1', 'TR_4');

-- ECGResult Table
INSERT INTO ECGResult (ECGResultID, TestResultID) VALUES
('ECG_1', 'TR_3');

-- CTScanResult Table
INSERT INTO CTScanResult (CTScanResultID, TestResultID) VALUES
('CTS_1', 'TR_5'),
('CTS_2', 'TR_6');

-- UrineTestResult Table
INSERT INTO UrineTestResult (UrineTestResultID, TestResultID, PHLevel, GlucoseLevel, Urea, Gravity, Creatinine) VALUES
('UTR_1', 'TR_7', 7.4, 90, 50, 1.0506, 1.1);

-- CoagulationResult Table
INSERT INTO CoagulationResult (CoagulationResultID, TestResultID, BleedingTime, ClottingTime, ProthrombinTime, INR) VALUES
('CGR_1', 'TR_8', 5, 25, 26, 2.5);

-- RenalFunctionResult Table
INSERT INTO RenalFunctionResult (RenalFunctionResultID, TestResultID, GFR_Rate, SerumCreatinine, UricAcid, Sodium, BloodUreaNitrogen) VALUES
('RFR_1', 'TR_15', 80, 1.1, 5, 140, 10),
('RFR_2', 'TR_16', 66, 0.9, 7, 144, 20);

-- LiverFunctionResult Table
INSERT INTO LiverFunctionResult (LiverFunctionResultID, TestResultID, AlanineAminotransferase, Albumin, AlkalinePhosphatase, AspartateAminotransferase, ConjugatedBilirubin) VALUES
('LFR_1', 'TR_9', 70, 4, 50, 20, 0.1);

-- RoutineHematologyResult Table
INSERT INTO RoutineHematologyResult (RoutineHematologyResultID, TestResultID, MCV, Lymphocyte, RBC, Platelets, WBC) VALUES
('RHR_1', 'TR_10', 90, 2000, 3.22, 567000, 5000),
('RHR_2', 'TR_11', 85, 3465, 5.1, 600000, 7865);

-- TumorMarkerResult Table
INSERT INTO TumorMarkerResult (TumorMarkerResultID, TestResultID, CancerAntigen, CA27_29, CA125, CarcinoembryonicAntigen, CirculatingTumorCells) VALUES
('TMR_1', 'TR_12', 50, 30, 50, 1.5, 20);

-- PancreasFunctionResult Table
INSERT INTO PancreasFunctionResult (PancreasFunctionResultID, TestResultID, Insulin, FastingGlucose, Lipase, Amylase, C_Peptide) VALUES
('PFR_1', 'TR_13', 2.5, 80, 100, 40, 1.5),
('PFR_2', 'TR_14', 3, 77, 20, 80, 1.3);

-- EndocrinologyResult Table
INSERT INTO EndocrinologyResult (EndocrinologyResultID, TestResultID, Throtropin, Testosterone, GrowthHormone, Insulin, Cortisol) VALUES
('ECR_1', 'TR_17', 4.22, 450, 2, 10, 500);
