# SQL-code
-- Address Table
CREATE TABLE Address (
    AddressID VARCHAR(10) PRIMARY KEY,
    Street VARCHAR(35),
    City VARCHAR(35),
    PostalCode VARCHAR(35)
);




-- Contact Table
CREATE TABLE Contact (
    ContactID VARCHAR(10) PRIMARY KEY,
    Phone VARCHAR(15),
    Email VARCHAR(50)
);



-- individual role tables

CREATE TABLE Admin (
    AdminID VARCHAR(10) PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    AddressID VARCHAR(10),
    ContactID VARCHAR(10),
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);


CREATE TABLE Staff (
    StaffID VARCHAR(10) PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    AddressID VARCHAR(10),
    ContactID VARCHAR(10),
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);


CREATE TABLE Doctor (
    DoctorID VARCHAR(10) PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    AddressID VARCHAR(10),
    ContactID VARCHAR(10),
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);


CREATE TABLE Patient (
    PatientID VARCHAR(10) PRIMARY KEY,
    Firstname VARCHAR(35),
    Lastname VARCHAR(35),
    DOB DATE,
    Sex VARCHAR(10),
    AddressID VARCHAR(10),
    ContactID VARCHAR(10),
    EmergencyContact VARCHAR(15),
    FOREIGN KEY (AddressID) REFERENCES Address(AddressID),
    FOREIGN KEY (ContactID) REFERENCES Contact(ContactID)
);


-- user tables
CREATE TABLE UserAccount (
    UserAccountID VARCHAR(10) PRIMARY KEY,
    UserType ENUM('Admin', 'Staff', 'Doctor', 'Patient'),
    Username VARCHAR(35),
    Password VARCHAR(35),
    ProfilePicture BLOB
    -- No FOREIGN KEY constraVARCHAR(10) on UserID
);


-- assigned tests and assigned blood tests table

CREATE TABLE AssignedTest (
    AssignedTestID VARCHAR(10) PRIMARY KEY,
    PatientID VARCHAR(10),
    DoctorID VARCHAR(10),
    DateAssigned DATE,
    TestType ENUM('X-Ray', 'Ultrasound', 'CTScan', 'ECG', 'UrineTest'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);


CREATE TABLE AssignedBloodTest (
    AssignedBloodTestID VARCHAR(10) PRIMARY KEY,
    PatientID VARCHAR(10),
    DoctorID VARCHAR(10),
    DateAssigned DATE,
    BloodTestType ENUM('RenalFunction', 'LiverFunction', 'RoutineHematology', 'Coagulation', 'RoutineChemistry', 'TumorMarker', 'Endocrinology', 'PancreasFunction'),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID)
);


-- the main test result table
-- stucture: assigned test -> test result -> individual test result table.
CREATE TABLE TestResult (
    TestResultID VARCHAR(10) PRIMARY KEY,
    StaffID VARCHAR(10),
    PatientID VARCHAR(10),
    AssignedTestID VARCHAR(10),        -- Used for general tests (e.g., X-Ray, ECG)
    AssignedBloodTestID VARCHAR(10),    -- Used for blood-related tests
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
    TestResultID VARCHAR(10) PRIMARY KEY,
    Image BLOB,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
-- individual results tables
CREATE TABLE XRayResult (
    XRayResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE UltrasoundResult (
    UltrasoundResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE CTScanResult (
    CTScanResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE ECGResult (
    ECGResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE UrineTestResult (
    UrineTestResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    PHLevel VARCHAR(10),
    GlucoseLevel DOUBLE,
    Urea DOUBLE,
    Gravity DOUBLE,
    Creatinine DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
-- all blood tests result tables
CREATE TABLE RenalFunctionResult (
    RenalFunctionResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    GFR_Rate DOUBLE,
    SerumCreatinine DOUBLE,
    UricAcid DOUBLE,
    Sodium DOUBLE,
    BloodUreaNitrogen DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE LiverFunctionResult (
    LiverFunctionResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    AlanineAminotransferase DOUBLE,
    Albumin DOUBLE,
    AlkalinePhosphatase DOUBLE,
    AspartateAminotransferase DOUBLE,
    ConjugatedBilirubin DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE RoutineHematologyResult (
    RoutineHematologyResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    MCV DOUBLE,
    Lymphocyte DOUBLE,
    RBC DOUBLE,
    Platelets DOUBLE,
    WBC DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE CoagulationResult (
    CoagulationResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    BleedingTime TIME,
    ClottingTime TIME,
    ProthrombVARCHAR(10)ime TIME,
    INR DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE RoutineChemistryResult (
    RoutineChemistryResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    CalciumIons DOUBLE,
    PotassiumIons DOUBLE,
    SodiumIons DOUBLE,
    Bicarbonate DOUBLE,
    ChlorideIons DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE TumorMarkerResult (
    TumorMarkerResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    CancerAntigen DOUBLE,
    CA27_29 DOUBLE,
    CA125 DOUBLE,
    CarcinoembryonicAntigen DOUBLE,
    CirculatingTumorCells DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE EndocrinologyResult (
    EndocrinologyResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    Throtropin DOUBLE,
    Testosterone DOUBLE,
    GrowthHormone DOUBLE,
    Insulin DOUBLE,
    Cortisol DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);
CREATE TABLE PancreasFunctionResult (
    PancreasFunctionResultID VARCHAR(10) PRIMARY KEY,
    TestResultID VARCHAR(10),
    Insulin DOUBLE,
    FastingGlucose DOUBLE,
    Lipase DOUBLE,
    Amylase DOUBLE,
    C_Peptide DOUBLE,
    FOREIGN KEY (TestResultID) REFERENCES TestResult(TestResultID)
);







