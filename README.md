# SQL-code
-- Address Table
CREATE TABLE Address (
    AddressID INT PRIMARY KEY,
    Street VARCHAR(35),
    City VARCHAR(35),
    PostalCode VARCHAR(35)
);

DELIMITER //
CREATE TRIGGER address_id_trigger BEFORE INSERT ON Address
FOR EACH ROW
BEGIN
    SET NEW.AddressID = CONCAT('ADR_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(AddressID, 5) AS UNSIGNED)) + 1 FROM Address), 1));
END;
//
DELIMITER ;


-- Contact Table
CREATE TABLE Contact (
    ContactID INT PRIMARY KEY,
    Phone VARCHAR(15),
    Email VARCHAR(50)
);

DELIMITER //
CREATE TRIGGER contact_id_trigger BEFORE INSERT ON Contact
FOR EACH ROW
BEGIN
    SET NEW.ContactID = CONCAT('CNT_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(ContactID, 5) AS UNSIGNED)) + 1 FROM Contact), 1));
END;
//
DELIMITER ;

-- individual role tables

CREATE TABLE Admin (
    AdminID INT PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    AddressID INT,
    ContactID INT,
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);
DELIMITER //
CREATE TRIGGER admin_id_trigger BEFORE INSERT ON Admin
FOR EACH ROW
BEGIN
    SET NEW.AdminID = CONCAT('ADM_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(AdminID, 5) AS UNSIGNED)) + 1 FROM Admin), 1));
END;
//
DELIMITER ;

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
DELIMITER //
CREATE TRIGGER staff_id_trigger BEFORE INSERT ON Staff
FOR EACH ROW
BEGIN
    SET NEW.StaffID = CONCAT('STF_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(StaffID, 5) AS UNSIGNED)) + 1 FROM Staff), 1));
END;
//
DELIMITER ;

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
DELIMITER //
CREATE TRIGGER doctor_id_trigger BEFORE INSERT ON Doctor
FOR EACH ROW
BEGIN
    SET NEW.DoctorID = CONCAT('DOC_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(DoctorID, 5) AS UNSIGNED)) + 1 FROM Doctor), 1));
END;
//
DELIMITER ;

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
DELIMITER //
CREATE TRIGGER patient_id_trigger BEFORE INSERT ON Patient
FOR EACH ROW
BEGIN
    SET NEW.PatientID = CONCAT('PAT_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(PatientID, 5) AS UNSIGNED)) + 1 FROM Patient), 1));
END;
//
DELIMITER ;

-- user tables
CREATE TABLE UserAccount (
    UserAccountID INT PRIMARY KEY,
    UserType ENUM('Admin', 'Staff', 'Doctor', 'Patient'),
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB
    -- No FOREIGN KEY constraint on UserID
);
DELIMITER //
CREATE TRIGGER useraccount_id_trigger BEFORE INSERT ON UserAccount
FOR EACH ROW
BEGIN
    SET NEW.UserAccountID = CONCAT('USR_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(UserAccountID, 5) AS UNSIGNED)) + 1 FROM UserAccount), 1));
END;
//
DELIMITER ;

-- assigned tests and assigned blood tests table

CREATE TABLE AssignedTest (
    AssignedTestID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    DateAssigned DATE,
    TestType ENUM('X-Ray', 'Ultrasound', 'CTScan', 'ECG'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);
DELIMITER //
CREATE TRIGGER assignedtest_id_trigger BEFORE INSERT ON AssignedTest
FOR EACH ROW
BEGIN
    SET NEW.AssignedTestID = CONCAT('AST_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(AssignedTestID, 5) AS UNSIGNED)) + 1 FROM AssignedTest), 1));
END;
//
DELIMITER ;

CREATE TABLE AssignedBloodTest (
    AssignedBloodTestID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    DateAssigned DATE,
    BloodTestType ENUM('RenalFunction', 'LiverFunction', 'RoutineHematology', 'Coagulation', 'RoutineChemistry', 'TumorMarker', 'Endocrinology', 'PancreasFunction'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);
DELIMITER //
CREATE TRIGGER assignedbloodtest_id_trigger BEFORE INSERT ON AssignedBloodTest
FOR EACH ROW
BEGIN
    SET NEW.AssignedBloodTestID = CONCAT('ABT_', COALESCE((
        SELECT MAX(CAST(SUBSTRING(AssignedBloodTestID, 5) AS UNSIGNED)) + 1 FROM AssignedBloodTest), 1));
END;
//
DELIMITER ;

-- the main test result table
-- stucture: assigned test -> test result -> individual test result table.
CREATE TABLE TestResult (
    TestResultID INT PRIMARY KEY,
    StaffID INT,
    PatientID INT,
    AssignedTestID INT,        -- Used for general tests (e.g., X-Ray, ECG)
    AssignedBloodTestID INT,    -- Used for blood-related tests
    DateUpdated DATE,
    DoctorNote VARCHAR(500),
    AbnormalResult BOOLEAN,
    FOREIGN KEY (AssignedTestID) REFERENCES AssignedTest(AssignedTestID),
    FOREIGN KEY (AssignedBloodTestID) REFERENCES AssignedBloodTest(AssignedBloodTestID),
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID)
);
-- image results for the ecg, ultrasound, ct scan, xray
CREATE TABLE ImagingResultDetails (
    TestResultID INT PRIMARY KEY,
    Image BLOB,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
-- individual results tables
CREATE TABLE XRayResult (
    XRayResultID INT PRIMARY KEY,
    TestResultID INT,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE UltrasoundResult (
    UltrasoundResultID INT PRIMARY KEY,
    TestResultID INT,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE CTScanResult (
    CTScanResultID INT PRIMARY KEY,
    TestResultID INT,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE ECGResult (
    ECGResultID INT PRIMARY KEY,
    TestResultID INT,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE UrineTestResult (
    UrineTestResultID INT PRIMARY KEY,
    TestResultID INT,
    PHLevel INT,
    GlucoseLevel DOUBLE,
    Urea DOUBLE,
    Gravity DOUBLE,
    Creatinine DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
-- all blood tests result tables
CREATE TABLE RenalFunctionResult (
    RenalFunctionResultID INT PRIMARY KEY,
    TestResultID INT,
    GFR_Rate DOUBLE,
    SerumCreatinine DOUBLE,
    UricAcid DOUBLE,
    Sodium DOUBLE,
    BloodUreaNitrogen DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE LiverFunctionResult (
    LiverFunctionResultID INT PRIMARY KEY,
    TestResultID INT,
    AlanineAminotransferase DOUBLE,
    Albumin DOUBLE,
    AlkalinePhosphatase DOUBLE,
    AspartateAminotransferase DOUBLE,
    ConjugatedBilirubin DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE RoutineHematologyResult (
    RoutineHematologyResultID INT PRIMARY KEY,
    TestResultID INT,
    MCV DOUBLE,
    Lymphocyte DOUBLE,
    RBC DOUBLE,
    Platelets DOUBLE,
    WBC DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE CoagulationResult (
    CoagulationResultID INT PRIMARY KEY,
    TestResultID INT,
    BleedingTime TIME,
    ClottingTime TIME,
    ProthrombinTime TIME,
    INR DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE RoutineChemistryResult (
    RoutineChemistryResultID INT PRIMARY KEY,
    TestResultID INT,
    CalciumIons DOUBLE,
    PotassiumIons DOUBLE,
    SodiumIons DOUBLE,
    Bicarbonate DOUBLE,
    ChlorideIons DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE TumorMarkerResult (
    TumorMarkerResultID INT PRIMARY KEY,
    TestResultID INT,
    CancerAntigen DOUBLE,
    CA27_29 DOUBLE,
    CA125 DOUBLE,
    CarcinoembryonicAntigen DOUBLE,
    CirculatingTumorCells DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE EndocrinologyResult (
    EndocrinologyResultID INT PRIMARY KEY,
    TestResultID INT,
    Throtropin DOUBLE,
    Testosterone DOUBLE,
    GrowthHormone DOUBLE,
    Insulin DOUBLE,
    Cortisol DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE PancreasFunctionResult (
    PancreasFunctionResultID INT PRIMARY KEY,
    TestResultID INT,
    Insulin DOUBLE,
    FastingGlucose DOUBLE,
    Lipase DOUBLE,
    Amylase DOUBLE,
    C_Peptide DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);







