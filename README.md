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




