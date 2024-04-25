DROP SCHEMA IF EXISTS `PROYECTO_AlonsoMauricio`;
CREATE SCHEMA IF NOT EXISTS `PROYECTO_AlonsoMauricio`;
USE PROYECTO_AlonsoMauricio;

CREATE TABLE IF NOT EXISTS Empresa (
    idEmpresa INT AUTO_INCREMENT,
    nombre_soc VARCHAR(100) NOT NULL,
    tipo VARCHAR(20) NOT NULL,
    cuit VARCHAR(13) NOT NULL,
    fecha_contrato DATE NOT NULL,
    envergadura ENUM ('pequeña', 'mediana', 'grande') NOT NULL,
    Fecha_I_M_E DATE, 
    PRIMARY KEY (idEmpresa)
);

CREATE TABLE IF NOT EXISTS DEP (
    idDEP INT AUTO_INCREMENT,
    nombre_dep VARCHAR(100) NOT NULL,
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY (idDEP)
);

CREATE TABLE IF NOT EXISTS Asesor (
    idAsesor INT AUTO_INCREMENT,
    nombre_asesor VARCHAR(100) NOT NULL,
    telefono VARCHAR(10),
    email VARCHAR(50) NOT NULL,
    idEmpresa INT,
    idDEP INT,  
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    FOREIGN KEY (idDEP) REFERENCES DEP(idDEP),  
    PRIMARY KEY (idAsesor)
);

CREATE TABLE IF NOT EXISTS Pais (
    idPais INT AUTO_INCREMENT ,
    nombre_pais VARCHAR(3) NOT NULL,
    PRIMARY KEY (idPais)
);


CREATE TABLE IF NOT EXISTS Representante (
    idRepresentante INT AUTO_INCREMENT,
    nombre_rep VARCHAR(100) NOT NULL,
    dni VARCHAR(8) NOT NULL,
    idPais INT NOT NULL,
    profesion VARCHAR(50),
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    FOREIGN KEY (idPais) REFERENCES Pais(idPais),
    PRIMARY KEY(idRepresentante)
);

CREATE TABLE IF NOT EXISTS Contacto (
    idContacto INT AUTO_INCREMENT,
    telefono VARCHAR(10),
    email VARCHAR(50) NOT NULL,
    web VARCHAR(100),
    idRepresentante INT,
    idEmpresa INT,
    idDEP INT,
    FOREIGN KEY (idRepresentante) REFERENCES Representante(idRepresentante),
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    FOREIGN KEY (idDEP) REFERENCES DEP(idDEP),
    PRIMARY KEY(idContacto)
);


CREATE TABLE IF NOT EXISTS Provincia(
    idProvincia INT AUTO_INCREMENT,
    nombre_provincia VARCHAR(15) NOT NULL,
    idPais INT,
    FOREIGN KEY (idPais) REFERENCES Pais(idPais),
    PRIMARY KEY(idProvincia)
);

CREATE TABLE IF NOT EXISTS Departamento (
    idDepartamento INT AUTO_INCREMENT,
    nombre_departamento VARCHAR(50) NOT NULL,
    idProvincia INT,
    FOREIGN KEY (idProvincia) REFERENCES Provincia(idProvincia),
    PRIMARY KEY(idDepartamento)
);

CREATE TABLE IF NOT EXISTS Localizacion (
    idLocalizacion INT AUTO_INCREMENT,
    domicilio VARCHAR(150) NOT NULL,
    localidad VARCHAR(100) NOT NULL,
    idDepartamento INT,
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    FOREIGN KEY (idDepartamento) REFERENCES Departamento(idDepartamento),
    PRIMARY KEY(idLocalizacion)
);



CREATE TABLE IF NOT EXISTS Bancos (
    idBanco INT AUTO_INCREMENT,
    banco VARCHAR(30),
    bcra ENUM ('1', '2', '3', '4', '5', '6'),
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY (idBanco)
);

CREATE TABLE IF NOT EXISTS Rrhh (
    idRrhh INT AUTO_INCREMENT,
    num_socios TINYINT NOT NULL,
    num_empleados TINYINT,
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY(idRrhh)
);

CREATE TABLE IF NOT EXISTS Ratio (
    idRatio INT AUTO_INCREMENT,
    patneto INT NOT NULL,
    margen DECIMAL NOT NULL,
    roa DECIMAL,
    roe DECIMAL,
    endeudamiento DECIMAL,
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY(idRatio)
);

CREATE TABLE IF NOT EXISTS Sector (
    idSector INT AUTO_INCREMENT,
    nombre_sector ENUM ('Industrial', 'Turismo', 'Mineria', 'Servicios', 'Apicultura', 'Agropecuario') NOT NULL,
    PRIMARY KEY(idSector)
);

CREATE TABLE IF NOT EXISTS Rubro (
    idRubro INT AUTO_INCREMENT,
    actividad VARCHAR(200) NOT NULL,
    idSector INT,
    idEmpresa INT,
    FOREIGN KEY (idSector) REFERENCES Sector(idSector),
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY(idRubro)
);

CREATE TABLE IF NOT EXISTS Programa (
    idPrograma INT AUTO_INCREMENT,
    nombre_programa VARCHAR(100) NOT NULL,
    descripcion TEXT,
    idEmpresa INT,
    FOREIGN KEY (idEmpresa) REFERENCES Empresa(idEmpresa),
    PRIMARY KEY (idPrograma)
);



----------


COMISIÓN: 53190
ALUMNO:   ALONSO MAURICIO JAVIER 
TRABAJO PRÁCTICO – ENTREGA 1

1 – El Consejo Federal de Desarrollo

El Consejo Federal de Desarrollo, cuyas siglas son CFD, (entidad real cuyo nombre es ficticio), es un organismo Federal que promueve el desarrollo de las provincias argentinas. Las provincias son las dueñas y las que dirigen el  CFD. 
La empresa cuenta con su sede central en la Ciudad Autónoma de Buenos Aires. Y opera a través de sus sucursales, denominadas Delegaciones Ejecutoras Provinciales, cuyas siglas son DEP, distribuidas en todas las capitales provinciales. 
Una de las principales funciones del CFD es asistir, mediante programas de desarrollo y créditos, a empresas. Las empresas deben iniciar los trámites en la sucursal perteneciente a su provincia. Y, para gozar de los beneficios del CFD, las empresas deben desarrollar actividades pertenecientes a sectores industriales, agropecuarias, mineras, turísticas y exportadoras. 
Las  DEP cuentan con gran autonomía, tienen personal propio y canales de comunicación individualizados. Pero responden a la sede Central ubicada en la Ciudad autónoma de Buenos Aires. 

2 – El problema a resolver

Un problema que preocupa a los directivos en la sede central del CFD es la fragmentación de la información. 
Históricamente, cada DEP ha gestionado de manera autónoma los datos de las empresas que solicitaron la asistencia del CFI.  Esto derivó a una gran heterogeneidad en soportes para datos, multiplicidad de modelos. Y, en muchos casos, el manejo de la información no está profesionalizado, almacenándose en tablas planas, generalmente en soporte Excel o incluso en papel. 
Cuando la sede central decide hacer una recopilación de la información encuentra grandes dificultades; no sólo es difícil y lento reunirla, también es arduo procesarla. 

3 – Nuestra misión 
El CFD nos expone el problema y nos solicita que diseñemos un modelo de datos que facilite y profesionalice el registro de la información recopilada en cada DEP. Y, al mismo tiempo, que quede estandarizada para su procesamiento y análisis. 

4 - El modelo de datos propuesto
Se desarrolló un modelo, utilizando el lenguaje SQL, y empleando para ello el sistema gestor MySQL. 
El modelo previsto cuenta con las siguientes tablas:
a)	Tabla de EMPRESA (la principal). 
En esta tabla se registran los datos principales de identificación de cada empresa asistida.
•	idEmpresa: A cada empresa se le asigna un ID único.
•	nombre_soc: es el nombre de fantasía que tiene la empresa.
•	cuit: hace referencia a la CUIT, la clave única de identificación tributaria. 
•	fecha_contrato: referencia a la fecha de constitución del contrato social, o estatuo, que regula la sociedad. 
•	envergadura: hace referencia al tamaño que tiene la empresa, si es pequeña (microempresa), mediana (pyme) o gran empresa.
•	Fecha_I_M_E: campo especial para informar la fecha de carga / actualización de cada registro.

b)	Tabla de BANCOS.
En esta tabla se detalla la información relativa al banco principal con el que opera la empresa. A través de ese banco se monetizan los créditos otorgados.
•	idBanco: a cada banco se le asigna un ID único.
•	bcra: puntaje asignado en la central de deudores del Banco Central de la República Argentina, siendo 1 la situación normal de cumplimiento y 6 una acreencia irrecuperable.
Esta tabla se relaciona con la tabla de empresas, porque registra una situación específica de BCRA por cada empresa.

c)	Tabla PAÍS:
Esta tabla está prevista para registrar la nacionalidad de los representantes de las empresas, ya que las empresas beneficiadas deben tener siempre sede en Argentina.
•	idPais: a cada país se le asigna un ID único.
•	nombre_país: se indican las siglas del país, siguiendo la codificación ALFA-3.


d)	Tabla PROVINCIA:
Esta tabla está pensada para registrar todas las provincias.
•	nombre_provincia: el nombre completo de la provincia
Esta tabla se relaciona con la tabla país. 
e)	Tabla DEPARTAMENTO
En esta tabla se registran los departamentos, que son las subdivisiones territoriales de cada provincia
•	idDepartament: registra el id de cada registro de esta tabla. 
•	nombre_departamento: previsto para el registro de cada nombre departamental. 
Esta tabla se relaciona con la tabla provincia, ya que los departamentos son subdivisiones de cada provincia. 
f)	Tabla LOCALIZACION
En esta tabla se registra la localización de la empresa
•	idLocalizacion: asigna un id de cada registro de esta tabla. 
•	domicilio: es el domicilio legal
•	localidad: la localidad donde está radicada la empresa
Se relaciona con la tabla empresa (el domicilio de cada empresa) y con la tabla departamento (donde está ubicada la empresa).
g)	Tabla REPRESENTANTE
Recopila información sobre el representante es el responsable y referente legal designado por cada empresa
•	idRepresentante: asigna un id de cada registro de esta tabla. 
•	nompre_rep: el nombre de cada representante
•	dni: el DNI de cada representante
Esta tabla se relaciona con la tabla empresa (cada empresa tiene un representante), con la tabla país (por la nacionalidad) y con la tabla contactos (tel, mail, web). 
h)	Tabla CONTACTO
Refleja los contactos tanto para la empresa, el representante y la sede provincial DEP 
•	idContacto: asigna un id de cada registro de esta tabla. 
•	telefono: para el número de teléfono.
•	email: para el correo electrónico.
•	we: para la página web.
Se relaciona con las tablas empresa, representante y dep. 


i)	Tabla RRHH
Esta tabla recopila la cantidad de socios y de empleados que trabaja en cada empresa. 
•	idRrhh: asigna un id de cada registro de esta tabla. 
•	num_socios: la cantidad de socios que tiene una empresa.
•	num_empleados: la cantidad de empleados que tiene una empresa. 
Se relaciona con la tabla empresa.
j)	Tabla RATIO
Esta tabla recopila indicadores financieros y contables para cada empresa.
•	idRatio: asigna un id de cada registro de esta tabla. 
•	patneto: registra el patrimonio neto de cada empresa.
•	margen: registra el margen del último balance de cada empresa (ventas - costos totales).
•	roa: registra el beneficio neto sobre el total de activos.
•	roe: registra el beneficio neto sobre el total del patrimonio neto.
•	endeudamiento: registra el nivel del endeudamiento en relación al activo total.
Se relaciona con la tabla empresa.
k)	Tabla SECTOR
Esta tabla registra los diferentes sectores financiados por la línea de créditos del CFD.
•	idSector: asigna un id de cada registro de esta tabla. 
•	nombre_sector: se registra cada sector financiable. 

l)	Tabla RUBRO
Esta tabla registra el rubro, o actividad, que desarrolla cada empresa
•	idRubro: asigna un id de cada registro de esta tabla. 
•	actividad: detalla la actividad específica que realiza una empresa.
Se relaciona con la tabla sector y empresa. 
m)	Tabla PROGRAMA
Describe cada programa, línea de créditos que está otorga el CFD.
•	idPrograma: asigna un id de cada registro de esta tabla. 
•	nombre_programa: el programa 
•	descripción: la descripción del programa, puede contener textos amplios con las características de dicha línea.
•	Se relaciona con la tabla empresa.



n)	Tabla ASESOR
Los asesores son el personal que trabaja en cada DEP y a su vez cada asesor tiene a su cargo el registro de una empresa.
•	idAsesor: asigna un id de cada registro de esta tabla. 
•	telefono: el teléfono del asesor
•	email: el email del asesor
  Se relaciona con la tabla empresa (los asesores asisten a las empresas) y con la tabla dep (los asesores forman parte de las dep).

o)	Tabla DEP
Las DEP son las sucursales que el CFD tiene en todo el país. Cada DEP tiene registradas empresas y asesores. 
•	idDEP:
•	nombre_dep: nombre o denominación de la sucursal

