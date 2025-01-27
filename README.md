Patients ----< Appointments >---- Healthcare_Facilities ----< Doctors
   |                           |
  ID (PK)                 Facility_ID (FK)
  Name                      Patient_ID (FK)
  Age                       Doctor_ID (FK)
  Location                  Appointment_Date
  Income_Level


-- Create Patients Table
CREATE TABLE Patients (
    patient_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    location VARCHAR(255),
    income_level VARCHAR(50)
);

-- Create Healthcare Facilities Table
CREATE TABLE Healthcare_Facilities (
    facility_id INT PRIMARY KEY,
    name VARCHAR(100),
    location VARCHAR(255),
    type VARCHAR(50)
);

-- Create Doctors Table
CREATE TABLE Doctors (
    doctor_id INT PRIMARY KEY,
    name VARCHAR(100),
    specialty VARCHAR(50),
    facility_id INT,
    FOREIGN KEY (facility_id) REFERENCES Healthcare_Facilities(facility_id)
);

-- Create Appointments Table
CREATE TABLE Appointments (
    appointment_id INT PRIMARY KEY,
    patient_id INT,
    facility_id INT,
    doctor_id INT,
    appointment_date DATE,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (facility_id) REFERENCES Healthcare_Facilities(facility_id),
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
);


-- Insert sample data into Patients table
INSERT INTO Patients (patient_id, name, age, location, income_level)
VALUES
(1, 'Alice Johnson', 30, 'Rural Area A', 'Low'),
(2, 'Bob Smith', 45, 'Rural Area B', 'Middle'),
(3, 'Charlie Lee', 65, 'Urban Area C', 'High');

-- Insert sample data into Healthcare_Facilities table
INSERT INTO Healthcare_Facilities (facility_id, name, location, type)
VALUES
(1, 'Rural Clinic A', 'Rural Area A', 'Clinic'),
(2, 'Urban Hospital B', 'Urban Area C', 'Hospital'),
(3, 'Rural Clinic B', 'Rural Area B', 'Clinic');

-- Insert sample data into Doctors table
INSERT INTO Doctors (doctor_id, name, specialty, facility_id)
VALUES
(1, 'Dr. John Doe', 'General Practitioner', 1),
(2, 'Dr. Jane Smith', 'Pediatrician', 2),
(3, 'Dr. Sam Green', 'Surgeon', 3);

-- Insert sample data into Appointments table
INSERT INTO Appointments (appointment_id, patient_id, facility_id, doctor_id, appointment_date)
VALUES
(1, 1, 1, 1, '2025-01-10'),
(2, 2, 2, 2, '2025-01-12'),
(3, 3, 3, 3, '2025-01-15');

SELECT * FROM Patients
WHERE location LIKE '%Rural%';

SELECT h.name AS Facility, COUNT(a.patient_id) AS Patients_Served
FROM Healthcare_Facilities h
LEFT JOIN Appointments a ON h.facility_id = a.facility_id
GROUP BY h.name;

SELECT h.name AS Facility, d.specialty, COUNT(*) AS Specialty_Count
FROM Healthcare_Facilities h
JOIN Doctors d ON h.facility_id = d.facility_id
GROUP BY h.name, d.specialty
ORDER BY COUNT(*) DESC;


