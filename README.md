# SQL-code
-- Address Table
CREATE TABLE Address (
    AddressID INT PRIMARY KEY,
    Street VARCHAR(35),
    City VARCHAR(35),
    PostalCode VARCHAR(10)
);

-- Contact Table
CREATE TABLE Contact (
    ContactID INT PRIMARY KEY,
    Phone VARCHAR(15),
    Email VARCHAR(50)
);
/*
before normalization, the address and contact table were repeated and included in the each user table.
after normalization(2nf), the tables are separated and act as foreign keys wherever needed.
*/

-- Main Entities: Admin, Staff, Doctor, Patient

CREATE TABLE Admin (
    AdminID INT PRIMARY KEY,
    Username VARCHAR(35),
    Password VARCHAR(35),
    AddressID INT,
    ContactID INT,
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);

CREATE TABLE Staff (
    StaffID INT PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    AddressID INT,
    ContactID INT,
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);

CREATE TABLE Doctor (
    DoctorID INT PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    AddressID INT,
    ContactID INT,
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);

CREATE TABLE Patient (
    PatientID INT PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    Sex VARCHAR(10),
    AddressID INT,
    ContactID INT,
    EmergencyContact VARCHAR(15),
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);

-- user tables, discuss!
-- no link to the individual tables, can be specified on application basis
-- normalized
/*CREATE TABLE UserAccount (
    UserAccountID INT PRIMARY KEY,
    UserType ENUM('Admin', 'Staff', 'Doctor', 'Patient'),
    UserID INT,
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB
    -- No FOREIGN KEY constraint on UserID
);*/

-- user tables as individual tables, discuss!
-- linked and not normalized and highly redundant.
/*
CREATE TABLE AdminUserAccount (
    UserAccountID INT PRIMARY KEY,
    AdminID INT,
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB,
    FOREIGN KEY (AdminID) REFERENCES Admin(AdminID)
);

CREATE TABLE StaffUserAccount (
    UserAccountID INT PRIMARY KEY,
    StaffID INT,
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB,
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);

CREATE TABLE DoctorUserAccount (
    UserAccountID INT PRIMARY KEY,
    DoctorID INT,
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB,
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);

CREATE TABLE PatientUserAccount (
    UserAccountID INT PRIMARY KEY,
    PatientID INT,
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB,
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID)
);
*/

-- assigned tests and assigned blood tests table
-- separated tables for easy to follow and clean database
CREATE TABLE AssignedTest (
    AssignedTestID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    DateAssigned DATE,
    TestType ENUM('X-Ray', 'Ultrasound', 'CTScan', 'ECG'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);

CREATE TABLE AssignedBloodTest (
    AssignedBloodTestID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    DateAssigned DATE,
    BloodTestType ENUM('RenalFunction', 'LiverFunction', 'RoutineHematology', 'Coagulation', 'RoutineChemistry', 'TumorMarker', 'Endocrinology', 'PancreasFunction'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);
-- the main test result table
-- stucture: assigned test -> test result -> individual test result table.
CREATE TABLE TestResult (
    TestResultID INT PRIMARY KEY,
    AssignedTestID INT,        -- Used for general tests (e.g., X-Ray, ECG)
    AssignedBloodTestID INT,    -- Used for blood-related tests
    DateUpdated DATE,
    DoctorNote VARCHAR(500),
    AbnormalResult BOOLEAN,
    FOREIGN KEY (AssignedTestID) REFERENCES AssignedTest(AssignedTestID),
    FOREIGN KEY (AssignedBloodTestID) REFERENCES AssignedBloodTest(AssignedBloodTestID)
);



