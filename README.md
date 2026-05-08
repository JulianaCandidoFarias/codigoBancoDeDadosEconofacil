# codigoBancoDeDadosEconofacil


Create Database If Not Exists EconoFacilDB;

Use EconoFacilDB;


Create Table If Not Exists AppUser(
	AppUserID		Int				Not Null Auto_Increment,
	AppFirstName 	Varchar(20) 	Not Null,
	AppLastName 	Varchar(40)		Not Null,
    AppUserEmail 	Varchar(100) 	Not Null Unique,
	AppUserPhone 	Int(13) 		Not Null Unique,
    AppUserCPF 		Int(11) 		Not Null Unique,
    AppUserPassword Varchar(20)		Not Null,
    AppUserBirthday	Date 			Not Null,
    
    Primary Key(AppUserID)
);

Create Table If Not Exists AppLogin(
	AppLoginID			Int				Not Null Auto_Increment,
    AppUserTableID		Int				Not Null,
	UserEmail 			Varchar(100) 	Not Null Unique,
    UserPassword 		Varchar(16) 	Not Null,
    UserForgotPassword 	Boolean,
    
    Primary Key(AppLoginID),
	Foreign Key(AppUserTableID) References AppUser(AppUserID)
);


Create Table If Not Exists AppLoginForgotPassword(
	AppLoginForgotPasswordID		Int				Not Null Auto_Increment,
    AppLoginTableID					Int				Not Null,
    CodeSentOnEmail					Int				Not Null Unique,
    RedefinePassword			 	varchar(16)		Not null,
    
    Primary Key(AppLoginForgotPasswordID),
    Foreign Key(AppLoginTableID) References AppLogin(AppLoginID)
    
);

Create Table If Not Exists UserMoneyInfo(
	UserMoneyInfoID 	Int 			Auto_Increment Not Null,
    AppUserTableID		Int				Not Null,
    UserMoney 			Int(6)			Not Null,
    UserHaveJob			Boolean			Not Null,
    UserJob				Varchar(20)		Not Null,
    UserSalary			Int(6)			Not Null,
    
    Primary Key(UserMoneyInfoID),
    Foreign Key(AppUserTableID) References AppUser(AppUserID)
);

Create Table If Not Exists AppWeekReview(
	AppWeekReviewID		Int			Not Null Auto_increment,
    AppUserTableID		Int			Not Null,
	StartDay 			Date 		Not Null,
    EndDay 				Date 		Not Null,
    MoneyOnStart 		Int(6) 		Not Null,
    
    Primary Key(AppWeekReviewID),
    Foreign Key(AppUserTableID) References AppUser(AppUserID)
);

Create Table If Not Exists AppMonthReview(
	AppMonthReviewID	Int			Not Null Auto_increment,
    AppUserTableID		Int			Not Null,
	StartDay 			Date 		Not Null,
    EndDay 				Date 		Not Null,
    MoneyOnStart 		Int(6) 		Not Null,
    
    Primary Key(AppMonthReviewID),
    Foreign Key(AppUserTableID) References AppUser(AppUserID)
);


INSERT INTO AppUser (AppFirstName, AppLastName, AppUserEmail, AppUserPhone, AppUserCPF, AppUserPassword, AppUserBirthday) 
VALUES 
('João', 	'Silva', 	'joao.silva@email.com', 	119888877, 	111222333, 'senha123', '1990-05-15'),
('Maria', 	'Santos', 	'maria.santos@email.com', 	119777766, 	222333444, 'senha456', '1985-10-20'),
('Pedro', 	'Oliveira', 'pedro.oli@email.com', 		119666655, 	333444555, 'senha789', '2000-01-10'),
('Ana', 	'Costa', 	'ana.costa@email.com', 		119555544, 	444555666, 'senha321', '1995-12-05'),
('Lucas',	'Souza', 	'lucas.souza@email.com', 	119444433, 	555666777, 'senha654', '1998-07-25');


INSERT INTO AppLogin (AppUserTableID, UserEmail, UserPassword, UserForgotPassword) 
VALUES 
(1, 'joao.silva@email.com', 	'senha123', 	FALSE),
(2, 'maria.santos@email.com', 	'senha456', 	FALSE),
(3, 'pedro.oli@email.com', 		'senha789', 	TRUE),
(4, 'ana.costa@email.com', 		'senha321', 	FALSE),
(5, 'lucas.souza@email.com', 	'senha654', 	FALSE);


INSERT INTO UserMoneyInfo (AppUserTableID, UserMoney, UserHaveJob, UserJob, UserSalary) 
VALUES 
(1, 1500, 	TRUE, 	'Engenheiro', 	8000),
(2, 2500, 	TRUE, 	'Professora', 	4000),
(3, 500, 	FALSE, 	'Estudante', 	0),
(4, 10000, 	TRUE, 	'Médica', 		15000),
(5, 300, 	TRUE, 	'Vendedor', 	2500);


SELECT AppFirstName, AppLastName, AppUserEmail FROM AppUser;

SELECT UserJob, UserSalary FROM UserMoneyInfo;

SELECT AppFirstName, AppLastName, AppUserBirthday 
FROM AppUser 
ORDER BY AppLastName ASC, AppFirstName ASC;

SELECT AppUserTableID, UserJob, UserMoney 
FROM UserMoneyInfo 
WHERE UserHaveJob = FALSE;

SELECT UserEmail, UserForgotPassword 
FROM AppLogin 
WHERE UserForgotPassword = TRUE;

SELECT AppFirstName, AppLastName, AppUserBirthday 
FROM AppUser 
WHERE AppUserBirthday >= '1995-01-01' 
ORDER BY AppUserBirthday DESC;

SELECT U.AppFirstName, U.AppLastName, M.UserMoney, M.UserSalary 
FROM AppUser U
INNER JOIN UserMoneyInfo M ON U.AppUserID = M.AppUserTableID;

SELECT U.AppFirstName, M.UserJob, M.UserSalary 
FROM AppUser U
INNER JOIN UserMoneyInfo M ON U.AppUserID = M.AppUserTableID
WHERE M.UserSalary > 4000
ORDER BY M.UserSalary DESC;

SELECT U.AppFirstName, L.UserEmail, L.UserPassword, M.UserJob
FROM AppUser U
INNER JOIN AppLogin L ON U.AppUserID = L.AppUserTableID
INNER JOIN UserMoneyInfo M ON U.AppUserID = M.AppUserTableID
WHERE M.UserHaveJob = TRUE;

SELECT L.UserEmail, F.CodeSentOnEmail, F.RedefinePassword 
FROM AppLogin L
INNER JOIN AppLoginForgotPassword F ON L.AppLoginID = F.AppLoginTableID
ORDER BY L.UserEmail ASC;
