
# Hospital Management Database

This project defines a relational database schema for a hospital management system. The database contains multiple tables designed to manage patients, doctors, appointments, medical history, and other relevant data.

## Schema

The schema used for this project is:

```sql
CREATE SCHEMA IF NOT EXISTS hospital_management;
```

## Tables

### 1. `patient`
Stores patient information.

```sql
CREATE TABLE hospital_management.patient (
    email VARCHAR(50) PRIMARY KEY,
    password VARCHAR(30) NOT NULL,
    name VARCHAR(50) NOT NULL,
    address VARCHAR(60) NOT NULL,
    gender VARCHAR(20) NOT NULL
);
```

### 2. `medical_history`
Stores the medical history of patients.

```sql
CREATE TABLE hospital_management.medical_history (
    medical_history_id INT PRIMARY KEY,
    date DATE NOT NULL,
    conditions VARCHAR(100) NOT NULL,
    surgeries VARCHAR(100) NOT NULL,
    medication VARCHAR(100) NOT NULL
);
```

### 3. `doctor`
Stores doctor information.

```sql
CREATE TABLE hospital_management.doctor (
    email VARCHAR(50) PRIMARY KEY,
    gender VARCHAR(20) NOT NULL,
    password VARCHAR(30) NOT NULL,
    name VARCHAR(50) NOT NULL
);
```

### 4. `appointment`
Tracks appointment details.

```sql
CREATE TABLE hospital_management.appointment (
    appointment_id INT PRIMARY KEY,
    date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    status VARCHAR(15) NOT NULL
);
```

### 5. `patient_visits`
Links patients to appointments and records their concerns and symptoms.

```sql
CREATE TABLE hospital_management.patient_visits (
    patient VARCHAR(50) NOT NULL,
    appt SERIAL,
    concerns VARCHAR(40) NOT NULL,
    symptoms VARCHAR(40) NOT NULL,
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    PRIMARY KEY (patient, appt)
);
```

### 6. `schedule`
Stores doctors' working schedules.

```sql
CREATE TABLE hospital_management.schedule (
    schedule_id SERIAL UNIQUE,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    break_time TIME NOT NULL,
    day VARCHAR(20) NOT NULL,
    PRIMARY KEY (schedule_id, start_time, end_time, break_time, day)
);
```

### 7. `patients_history`
Links patients to their medical history.

```sql
CREATE TABLE hospital_management.patients_history (
    patient VARCHAR(50) NOT NULL,
    history SERIAL,
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history)
);
```

### 8. `diagnose`
Records diagnosis information for each appointment.

```sql
CREATE TABLE hospital_management.diagnose (
    appt SERIAL,
    doctor VARCHAR(50) NOT NULL,
    diagnosis VARCHAR(40) NOT NULL,
    prescription VARCHAR(50) NOT NULL,
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (appt, doctor)
);
```

### 9. `doctor_schedules`
Links doctors to their schedules.

```sql
CREATE TABLE hospital_management.doctor_schedules (
    sched SERIAL,
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (sched) REFERENCES hospital_management.schedule (schedule_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (sched, doctor)
);
```

### 10. `doctor_view_history`
Records which doctors viewed a patient's medical history.

```sql
CREATE TABLE hospital_management.doctor_view_history (
    history SERIAL,
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history, doctor)
);
```

## Additional Information

An additional schema is also created for an online retail application, which stores user login details.

```sql
CREATE SCHEMA IF NOT EXISTS online_retail_app;

CREATE TABLE online_retail_app.user_login (
    user_id TEXT PRIMARY KEY,
    user_password TEXT,
    first_name TEXT,
    last_name TEXT,
    sign_up_on DATE,
    email_id TEXT
);
```

## Running the SQL Script

To run the above SQL, use a PostgreSQL database instance. Make sure to execute the `DROP` commands first if the schema or tables already exist.

```
DROP SCHEMA IF EXISTS hospital_management CASCADE;
DROP TABLE IF EXISTS online_retail_app.user_login;
...
```

## License

This project is open-source and free to use.
```

This `README.md` covers the structure and purpose of each table, with SQL snippets and relevant descriptions. Let me know if you'd like any adjustments!
