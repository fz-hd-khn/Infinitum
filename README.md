# Infinitum
### User Stories
1. As a patient, want to be able to access important files and lab results to watch my medical progress. 
   
2. As a patient, I want to be able to track fitness goals and be able to have all health information synchronized so that it is in one convenient place
   
3. As a health care provider, I want to be able to upload documents safely and securely into a database where all documents are organized for ease of access and explanations for patients who want to remain up to date on their health. 

4. As a lab technician, I want to upload lab results into a singular database so that all files remain organized and labs can be completed in a timely matter. 

5. As a Telehealth provider, I want to be able to access files/lab results with ease so that information is up to date and appointments can be completed. 
   

 
### [FIGMA](https://www.figma.com/file/Hjr3E0o9ygcAWvzCje8waA/Untitled?node-id=0%3A1&t=U7jJoqYthjWmaSXo-1)

### Write-up

*Ad Infinitum*, meaning “to infinity” in latin. 

Everyone has had appointments or times where a lab result has been delivered to a specialist but your family doctor doesn’t have access to it or when you need to fill form after form when going to walk in clinics or a new doctor. 

*Infinitum* is meant to be a web app that revolutionizes managing your health care. It will rival current and future health apps that have downfalls such as: only certain physicians using them, needing a subscription and not having all of one’s health files in one place. *Infinitum* is meant to streamline your healthcare and improve health outcomes by providing a comprehensive platform, where appointments can be kept track of, prescriptions easily pulled up on your phone and lab results being at yours and your healthcare providers fingertips. 

*Infinitum* aims to improve communication between healthcare providers and patients.

*Infinitum*'s synchronized platform allows healthcare providers to easily access and cross-reference patients' medical records, reducing the risk of missed diagnoses or treatment errors and improving overall patient outcomes. Ease of upload of files also means traditional mechanical note taking and form filling won't be necessary saving time for more important matters.  

*Infinitum* is for the everyday person allowing them to be more responsible for their medical progress, and be part of it every step of the way, from tracking medications to fitness goals it is meant to help you be more involved in your healthcare. 


### MySQL Database
-- CREATE NEW DATABASE
CREATE SCHEMA infinitum2 ;

-- CREATE TABLES 
CREATE TABLE `infinitum2`.`PHYSICIAN` (
  `PHYS_ID` INT NOT NULL, 
  `PRAC_ID` INT NULL,
  `FIRST_NAME` VARCHAR(45) NULL,
  `LAST_NAME` VARCHAR(45) NULL,
  PRIMARY KEY (`PHYS_ID`));
  
  CREATE TABLE `infinitum2`.`MED_PRAC` (
  `PRAC_ID` INT NOT NULL,
  `PHYS_ID` INT NULL,
  `STREET_AD` VARCHAR(45) NULL,
  `PHONE_NUM` INT NULL,
  `POST_CD` VARCHAR(8) NULL,
  PRIMARY KEY (`PRAC_ID`));
  
CREATE TABLE `infinitum2`.`LAB_DIAG` (
  `CENTRE_ID` INT NOT NULL,
  `TECH_ID` INT NULL,
  `STREE_ADD` VARCHAR(45) NULL,
  `POST_CODE` VARCHAR(8) NULL,
  `PHONE_NUM` INT NULL,
  PRIMARY KEY (`CENTRE_ID`));
  CREATE TABLE `infinitum2`.`APPTS` (
  `APPT_ID` INT NOT NULL,
  `PHYS_ID` INT NULL,
  `APPT_DATE` DATETIME NULL,
  `STATUS` VARCHAR(45) NULL,
  PRIMARY KEY (`APPT_ID`));
  
CREATE TABLE `infinitum2`.`PATIENT` (
  `PAT_ID` INT NOT NULL,
  `CARD_NUM` VARCHAR(12) NULL,
  `FIRST_NAME` VARCHAR(45) NULL,
  `LAST_NAME` VARCHAR(45) NULL,
  `D_O_B` DATETIME NULL,
  `STREET_ADD` VARCHAR(45) NULL,
  `POST_CODE` VARCHAR(8) NULL,
  `CITY` VARCHAR(45) NULL,
  `PHONE_NUM` INT NULL,
  PRIMARY KEY (`PAT_ID`));
  
CREATE TABLE `infinitum2`.`PAT_FILE` (
  `FILE_ID` INT NOT NULL,
  `PAT_ID` INT NULL,
  `DESC` LONGBLOB NULL,
  `NOTES` LONGBLOB NULL,
  `LABS` LONGBLOB NULL,
  PRIMARY KEY (`FILE_ID`));
  
CREATE TABLE `infinitum2`.`PHYS_PAT` (
  `PHYS_ID` INT NULL,
  `PAT_ID` INT NULL);
  
  CREATE TABLE `infinitum2`.`PHYS_MEDPRAC` (
  `PHYS_ID` INT NULL,
  `PRAC_ID` INT NULL);
  
  -- ALTERED PHYS TABLE
ALTER TABLE `infinitum2`.`PHYSICIAN`
CHANGE COLUMN `PRAC_ID` `PRAC_ID` VARCHAR(20) NULL;

-- PHYS FOREIGN KEY ADDED 
ALTER TABLE `infinitum2`.`PHYSICIAN`
ADD CONSTRAINT `fk_prac_id` FOREIGN KEY (`PRAC_ID`)
  REFERENCES `infinitum`.`medical_practice` (`practice_id`)
  ON DELETE SET NULL
  ON UPDATE CASCADE;
  
-- MED PRAC FOREIGN KEY ADDED 
  ALTER TABLE `infinitum2`.`MED_PRAC`
ADD CONSTRAINT `fk_phys_id` FOREIGN KEY (`PHYS_ID`)
  REFERENCES `infinitum2`.`PHYSICIAN` (`PHYS_ID`)
  ON DELETE SET NULL
  ON UPDATE CASCADE;
  
  -- ALTER ADDED TO LAB-DIAG TABLE 
  ALTER TABLE `infinitum2`.`LAB_DIAG`
ADD COLUMN `RESULT` VARCHAR(255) NULL;

-- LAB-DIAG FOREIGN KEY ADDED 
  ALTER TABLE `infinitum2`.`LAB_DIAG`
ADD CONSTRAINT `fk_tech_id` FOREIGN KEY (`TECH_ID`)
  REFERENCES `infinitum2`.`TECHNICIAN` (`TECH_ID`) -- Reference the primary key in the TECHNICIAN table
  ON DELETE SET NULL
  ON UPDATE CASCADE;
  
-- APPOINTMENT FOREIGN KEY ADDED 
ALTER TABLE `infinitum2`.`APPTS`
ADD CONSTRAINT `fk_appts_phys_id` FOREIGN KEY (`PHYS_ID`)
  REFERENCES `infinitum2`.`PHYSICIAN` (`PHYS_ID`)
  ON DELETE SET NULL;
  
  ALTER TABLE `infinitum2`.`PATIENT`
ADD CONSTRAINT `fk_card_num` FOREIGN KEY (`CARD_NUM`)
  REFERENCES `infinitum2`.`HEALTH_CARD` (`CARD_NUM`)
  ON DELETE SET NULL
  ON UPDATE CASCADE;
  
  -- PAT FILE FOREIGN KEY ADDED 
ALTER TABLE `infinitum2`.`PAT_FILE`
ADD CONSTRAINT `fk_pat_id` FOREIGN KEY (`PAT_ID`)
  REFERENCES `infinitum2`.`PATIENT` (`PAT_ID`)
  ON DELETE SET NULL
  ON UPDATE CASCADE;
  
-- PHYICIAN TABLE DATA
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('1', '00011', 'THE', 'ROCK');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('2', '00012', 'KARL', 'MARX');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('3', '00013', 'HENRY', 'FORD');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('4', '00014', 'JASON', 'BORNE');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('5', '00015', 'JOHN ', 'WICK');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('6', '00016', 'MARIE', 'ANTOINETTE');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('7', '00017', 'KIM', 'KARDASHIAN');
INSERT INTO `infinitum2`.`PHYSICIAN` (`PHYS_ID`, `PRAC_ID`, `FIRST_NAME`, `LAST_NAME`) VALUES ('8', '00018', 'JUSTIN', 'TRUDEAU');

-- PATIENT TABLE DATA
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('0', '185923292874', 'KAT', 'JONES', '1992-01-01', '45 VAMPIRE WAY', 'L8H6G5', 'MISSISSAUGA', '9055678765');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('1', '728535703319', 'JACOB', 'SCOUSE', '1997-09-08', '654 CIRCLE ROAD', 'L0I8U7', 'TORNTO', '9056781234');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('2', '511967359571', 'FAIZA', 'KHAN', '1987-10-11', '76 VALLEY WAY', 'G65G5G', 'MISSISSAUGA', '4167895432');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('3', '577606322488', 'CALEB', 'FRITZ', '1964-03-09', '67 TREMBLENT AVE', 'P0O9I8', 'MISSISSAUGA', '4164566543');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('4', '355497775697', 'MOE', 'TIV', '1956-11-11', '12 BLOOR ST', 'Y6T7Y6', 'TORONTO', '6476785432');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('5', '094377316373', 'AIR', 'CON', '2002-02-02', '87 DUNDAS RD', 'W3R4E3', 'BRAMPTON', '4165133226');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('6', '361876218430', 'COMP', 'UTER', '2011-12-01', '86 EGLINTON ROAD', 'T6Y5F6', 'BRAMPTON', '5132349986');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('7', '204287307439', 'DINING', 'TABLE', '2020-05-31', '92 HURONTARIO ST', 'T7Y4R5', 'OAKVILLE', '6478975433');
INSERT INTO `infinitum2`.`PATIENT` (`PAT_ID`, `CARD_NUM`, `FIRST_NAME`, `LAST_NAME`, `D_O_B`, `STREET_ADD`, `POST_CODE`, `CITY`, `PHONE_NUM`) VALUES ('8', '705132871993', 'KEY', 'BOARD', '2021-01-02', '65 CONFEDERATION PKWY', 'L5A3S2', 'BARRIE', '9056674455');

-- PATIENT TABLE ALTERED 
ALTER TABLE `infinitum2`.`PATIENT`
CHANGE COLUMN `PHONE_NUM` `PHONE_NUM` BIGINT NULL;

-- SELECT TO VIEW ALL 
SELECT * FROM infinitum2.PATIENT; 

-- LAB-DIAG TABLE DATA
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('1', '3989007331', '65 TURNOVER WAY', 'L9K7J7 ', '9056754321', 'N/A');
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('2', '1160739897', '45 CASPER ST', '7Y65T6', '9054609899', 'N/A');
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('3', '2511719575', '1200 HURONTARIO ST', 'L0O9K9', '4166549900', 'N/A');
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('4', '7656114085', '2400 LAKESHORE RD', 'L8K9U8', '4168899000', 'N/A');
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('5', '7656114085', '45 DIXIE ROAD', 'J8H7U6', '6478456798', 'N/A');
INSERT INTO `infinitum2`.`LAB_DIAG` (`CENTRE_ID`, `TECH_ID`, `STREE_ADD`, `POST_CODE`, `PHONE_NUM`, `RESULT`) VALUES ('6', '1648054864', '65 BLOOR ST', 'J8U7Y7', '6477865786', 'N/A');

-- ALTER TABLE TO CHANGE PHONE AND TECH ID VALUES 
ALTER TABLE `infinitum2`.`LAB_DIAG`
  MODIFY `TECH_ID` BIGINT,
  MODIFY `PHONE_NUM` BIGINT;
  
  -- SELECT PHYS IN ORDER TO RELATE TO APPT TABLE FOR EASE OF CREATION OF TABLE 
  SELECT * FROM infinitum2.PHYSICIAN;
  
   -- ADD DATA INTO APPTS TABLE 
 INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('1', '1', '2023-05-05 13:00', 'CONFIRMED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('2', '2', '2023-01-12 15:00', 'CANCELLED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('3', '3', '2023-02-02 08:00', 'NOT CUNFIRMED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('4', '4', '2023-05-07 09:00', 'RESCHEDULED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('5', '5', '2023-05-02 10:00 ', 'CONFIRMED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('6', '6', '2023-08-08', 'NOT CONFIRMED');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('7', '7', '2023-09-08 07:00', 'NOT CONFIRMED ');
INSERT INTO `infinitum2`.`APPTS` (`APPT_ID`, `PHYS_ID`, `APPT_DATE`, `STATUS`) VALUES ('8', '8', '2023-12-12 10:00', 'NOT CONFIRMED');

-- UPDATED VALUE TO FIX ERROR IN ROW 5 
UPDATE `infinitum2`.`APPTS`
SET `APPT_DATE` = '2023-05-02 10:00'
WHERE `APPT_ID` = 5;

-- ADD DATA TO MED_PRAC TABLE 
INSERT INTO `infinitum2`.`MED_PRAC` (`PRAC_ID`, `PHYS_ID`, `STREET_AD`, `PHONE_NUM`, `POST_CD`) VALUES ('1', '1', '456 HURONTARIO ST', '9056785436', 'L8H6G5');
INSERT INTO `infinitum2`.`MED_PRAC` (`PRAC_ID`, `PHYS_ID`, `STREET_AD`, `PHONE_NUM`, `POST_CD`) VALUES ('2', '2', '675 BLOOR ST ', '9056765432', 'L0O9I8');
INSERT INTO `infinitum2`.`MED_PRAC` (`PRAC_ID`, `PHYS_ID`, `STREET_AD`, `PHONE_NUM`, `POST_CD`) VALUES ('3', '3', '675 DUNDAS ST ', '4167865678', 'M7H6G5');
INSERT INTO `infinitum2`.`MED_PRAC` (`PRAC_ID`, `PHYS_ID`, `STREET_AD`, `PHONE_NUM`, `POST_CD`) VALUES ('4', '4', '8899 LAKESHORE ROAD ', '6476675676', 'T8U7Y7');
INSERT INTO `infinitum2`.`MED_PRAC` (`PRAC_ID`, `PHYS_ID`, `STREET_AD`, `PHONE_NUM`, `POST_CD`) VALUES ('5', '5', '7686 EGLINTON AVE', '9054465674', 'M8G6H6');

-- CHANGE PHONE NUMBER COLUMN TO BIGINT 
ALTER TABLE `infinitum2`.`MED_PRAC`
MODIFY `PHONE_NUM` BIGINT UNSIGNED;

-- INSERT DATA INTO PAT FILE 
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('1', '1');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('2', '2');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('3', '3');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('4', '4');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('5', '5');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('6', '6');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('7', '7');
INSERT INTO `infinitum2`.`PAT_FILE` (`FILE_ID`, `PAT_ID`) VALUES ('8', '7');
 
 -- DESCRIBE PAT FILE DATA 
 DESCRIBE `infinitum2`.`PAT_FILE`;
 
 -- SELECT DIFFERENT PATIENT  
SELECT * FROM `infinitum2`.`PATIENT` WHERE `CARD_NUM` = '355497775697';
SELECT * FROM `infinitum2`.`PATIENT` WHERE `FIRST_NAME` = 'KAT' AND `LAST_NAME` = 'JONES';

-- SELECT PHYS
SELECT * FROM `infinitum2`.`PHYSICIAN` WHERE `FIRST_NAME` = 'THE' AND `LAST_NAME` = 'ROCK';

-- SELECT PAT FILES
SELECT * FROM `infinitum2`.`PAT_FILE` WHERE `FILE_ID` = 1;

-- SELECT PHYS WITH NAMES STARTING WITH 'J'
SELECT * FROM `PHYSICIAN` WHERE `FIRST_NAME` LIKE 'J%';

-- SELECT PATIENTS WHO LIVE IN MISSISSAUGA 
SELECT * FROM `PATIENT` WHERE `CITY` LIKE 'M%';

-- SELECT ALL PATIENTS BORN BETWEEN 1999-2023
SELECT * FROM `PATIENT` WHERE `D_O_B` BETWEEN '1999-01-01' AND '2023-12-31';

-- ADD PHONE NUMBER TO PHYS TABLE 
ALTER TABLE `PHYSICIAN` ADD COLUMN `PHONE_NUM` VARCHAR(15);

-- ADD DATA TO PHONE_NUM COLUMN 
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '4167862341' WHERE `PHYS_ID` = 1;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '4165641234' WHERE `PHYS_ID` = 2;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '9055687654' WHERE `PHYS_ID` = 3;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '6477892345' WHERE `PHYS_ID` = 4;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '6476541234' WHERE `PHYS_ID` = 5; 
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '2892328957' WHERE `PHYS_ID` = 6;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '5132347869' WHERE `PHYS_ID` = 7;
UPDATE `PHYSICIAN` SET `PHONE_NUM` = '6477894563' WHERE `PHYS_ID` = 8;

-- VIEW TABLE AFTER CHANGES 
SELECT * FROM PHYSICIAN; 

-- SELECT, IN WITH MED PRAC
SELECT * FROM MED_PRAC WHERE `STREET_AD` IN ("HURONTARIO", "DUNDAS"); 

-- GROUP BY MED PRAC LOCATION 
SELECT `STREET_AD`, `POST_CD`, COUNT(*) as `NUM_OF_MED_PRAC`
FROM `infinitum2`.`MED_PRAC`
GROUP BY `STREET_AD`, `POST_CD`;

-- ALTER TABLE FOR MED PRAC TO ADD CITY 
ALTER TABLE `infinitum2`.`MED_PRAC`
ADD COLUMN `CITY` VARCHAR(255);

-- ADD CITIES TO EACH MED PRAC 
UPDATE `infinitum2`.`MED_PRAC` SET `CITY` = 'MISSISSAUGA' WHERE (`PRAC_ID` = '1');
UPDATE `infinitum2`.`MED_PRAC` SET `CITY` = 'MISSISSAUGA ' WHERE (`PRAC_ID` = '2');
UPDATE `infinitum2`.`MED_PRAC` SET `CITY` = 'TORONTO' WHERE (`PRAC_ID` = '3');
UPDATE `infinitum2`.`MED_PRAC` SET `CITY` = 'BRAMPTON' WHERE (`PRAC_ID` = '4');
UPDATE `infinitum2`.`MED_PRAC` SET `CITY` = 'BRAMPTON ' WHERE (`PRAC_ID` = '5');

-- GROUP BY CITY, MEDPRAC
SELECT `CITY`, COUNT(*) as `NUM_OF_MED_PRAC`
FROM `infinitum2`.`MED_PRAC`
GROUP BY `CITY`;

-- ALTER APPTS TO ADD PAT_FILE AS NEW FOREIGN KEY 
ALTER TABLE `infinitum2`.`APPTS`
ADD COLUMN `PAT_ID` INT,
ADD CONSTRAINT `fk_appts_pat_id`
    FOREIGN KEY (`PAT_ID`)
    REFERENCES `infinitum2`.`PATIENT` (`PAT_ID`);

-- ADD INFO INTO PAT_ID COLUMN IN TABLE 
UPDATE `infinitum2`.`APPTS`
SET `PAT_ID` = 1
WHERE `APPT_ID` = 1;
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '3' WHERE (`APPT_ID` = '2');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '4' WHERE (`APPT_ID` = '3');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '3' WHERE (`APPT_ID` = '4');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '2' WHERE (`APPT_ID` = '5');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '3' WHERE (`APPT_ID` = '6');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '7' WHERE (`APPT_ID` = '7');
UPDATE `infinitum2`.`APPTS` SET `PAT_ID` = '6' WHERE (`APPT_ID` = '8');

-- JOIN BETWEEN PAT_FILE AND APPTS 
SELECT ap.`APPT_ID`, ap.`PHYS_ID`, ap.`APPT_DATE`, ap.`STATUS`, pf.`FILE_ID`, pf.`PAT_ID`
FROM `infinitum2`.`APPTS` AS ap, `infinitum2`.`PAT_FILE` AS pf
WHERE ap.`PAT_ID` = pf.`PAT_ID`;

-- JOIN PATIENT FILE AND PATIENT 
SELECT p.PAT_ID, p.FIRST_NAME, p.LAST_NAME, pf.FILE_ID, pf.DESC
FROM infinitum2.PATIENT p
JOIN infinitum2.PAT_FILE pf ON p.PAT_ID = pf.PAT_ID;

-- JOIN BETWEEN PATIENT AND PAT FILE 
SELECT P.PAT_ID, P.FIRST_NAME, P.LAST_NAME, PF.FILE_ID, PF.DESC
FROM infinitum2.PATIENT AS P, infinitum2.PAT_FILE AS PF
WHERE P.PAT_ID = PF.PAT_ID;

-- JOIN BETWEEN PHYSICIANS AND APPT
SELECT PHY.PHYS_ID, PHY.FIRST_NAME, PHY.LAST_NAME, A.APPT_ID, A.APPT_DATE, A.STATUS
FROM infinitum2.PHYSICIAN AS PHY, infinitum2.APPTS AS A
WHERE PHY.PHYS_ID = A.PHYS_ID;

