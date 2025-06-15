# Ασφάλεια ΒΔ: ORACLE Integrity Constraints

## Αρχέιο PDF [[ISS-10.pdf|εδώ]] ![[ISS-10.pdf]]
# Βασικά Σημεία
- **Σκοπός**: Εξασφάλιση της ποιότητας και ακεραιότητας των δεδομένων σε βάσεις δεδομένων
- **Παραδείγματα Πινάκων**: dept (τμήματα) και std (φοιτητές)
- **Πρόβλημα**: Χωρίς περιορισμούς μπορούν να εισαχθούν εσφαλμένα δεδομένα
# Βασικοί Τύποι Περιορισμών Ακεραιότητας
### Nulls
- Αποτρέπει την εισαγωγή κενών τιμών (null) σε συγκεκριμένη στήλη
- Εξασφαλίζει ότι κρίσιμα πεδία έχουν πάντα τιμές
### Unique Column Values
- Εξασφαλίζει μοναδικές τιμές σε μία ή περισσότερες στήλες
- Δημιουργεί μοναδικό κλειδί (unique key)
- Μπορεί να εφαρμοστεί σε σύνθετο μοναδικό κλειδί (composite unique key)
### Primary Key Values
- Κάθε γραμμή προσδιορίζεται μοναδικά από τις τιμές του κλειδιού
- Συνδυάζει τις λειτουργίες NOT NULL και UNIQUE
- Μόνο ένα primary key ανά πίνακα
### Referential Integrity (Foreign Key)
- **Foreign Key**: Στήλη που αναφέρεται σε κλειδί άλλου πίνακα
- **Referenced Key**: Το κλειδί στον γονικό πίνακα
- **Child Table**: Πίνακας με το foreign key (εξαρτώμενος)
- **Parent Table**: Πίνακας που περιέχει το referenced key
### Complex Integrity Checking (CHECK)
- Προσαρμοσμένες συνθήκες που ορίζει ο χρήστης
- Βασίζεται σε boolean expressions
- Επιτρέπει πολλαπλούς περιορισμούς στην ίδια στήλη
# Περιορισμοί της Oracle 
### Δηλωτικές Μέθοδοι DDL
- **NOT NULL**: Απαιτεί ύπαρξη τιμών
- **UNIQUE**: Μοναδικές τιμές, επιτρέπει nulls
- **PRIMARY KEY**: Συνδυασμός NOT NULL + UNIQUE
- **FOREIGN KEY**: Αναφορική ακεραιότητα
- **CHECK**: Προσαρμοσμένες συνθήκες
### Χαρακτηριστικά Oracle
- Αυτόματη δημιουργία index για UNIQUE constraints
- Μηνύματα λάθους για παραβίαση περιορισμών
- Rollback της εντολής σε περίπτωση παραβίασης
# Διαχείριση Περιορισμών
### Ενεργοποίηση/Απενεργοποίηση
- **ENABLE CONSTRAINT**: Ελέγχει όλες τις υπάρχουσες γραμμές
- **DISABLE CONSTRAINT**: Επιτρέπει παραβίαση περιορισμών
- **ENABLE NOVALIDATE**: Ελέγχει μόνο νέες/τροποποιημένες γραμμές
### Εντολές ALTER TABLE 
```sql
ALTER TABLE table_name DISABLE CONSTRAINT constraint_name;
ALTER TABLE table_name ENABLE CONSTRAINT constraint_name;
```
### Πλεονεκτήματα Περιορισμών Ακεραιότητας
- **Εύκολος ορισμός**: Με χρήση εντολών SQL
- **Κεντρική διαχείριση**: Αποθήκευση στο data dictionary
- **Παραγωγικότητα**: Εύκολη ενημέρωση κανόνων
- **Άμεση ανάδραση**: Μηνύματα λάθους της Oracle
- **Βελτίωση απόδοσης**: Query optimizer optimization
- **Ευελιξία**: Προσωρινή ακύρωση για bulk loading
# Πρακτικά Παραδείγματα
### Σωστή Δημιουργία Πινάκων
```sql
CREATE TABLE dept (
    DEPTcode NUMBER CONSTRAINT pk_DEPT PRIMARY KEY
                   CONSTRAINT limit_DEPTcode CHECK (DEPTcode BETWEEN 100 AND 999),
    DEPTdesc VARCHAR2(30) CONSTRAINT nn_DEPTdesc NOT NULL
                          CONSTRAINT uk_DEPTdesc UNIQUE
                          CONSTRAINT upper_DEPTdesc CHECK (DEPTdesc = UPPER(DEPTdesc))
);
```
### Τεστ Ακεραιότητας
- Έλεγχος με διάφορες INSERT εντολές
- Εντοπισμός παραβιάσεων περιορισμών
- Αυτόματη απόρριψη μη έγκυρων δεδομένων