CREATE TABLE departamentos (
    nombred     VARCHAR(30) NOT NULL,
    numerod     INT     NOT NULL,
    nssgte      CHAR(9)     NOT NULL,
    fechainicgte    DATE,
    PRIMARY KEY (numerod),
    UNIQUE (nombred)
);

CREATE TABLE empleados (
    nombrep     VARCHAR(30)     NOT NULL,
    inic        CHAR,
    apellido    VARCHAR(30)     NOT NULL,
    nss     CHAR(9)     NOT NULL,
    fechan      DATE,
    dirección   VARCHAR(30),
    sexo        CHAR,
    salario     DECIMAL(10,2),
    nsssuper    CHAR(9),
    nd      INT         NOT NULL,
    PRIMARY KEY (nss)
);

CREATE TABLE lugares_deptos (
    numerod     INT     NOT NULL,
    lugard      VARCHAR(30) NOT NULL,
    PRIMARY KEY (numerod, lugard),
    FOREIGN KEY (numerod) REFERENCES departamentos (numerod)
        ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE proyectos (
    nombrepr    VARCHAR(30) NOT NULL,
    numerop     INT     NOT NULL,
    lugarp      VARCHAR(30),
    numd        INT     NOT NULL,
    PRIMARY KEY (numerop),
    FOREIGN KEY (numd) REFERENCES departamentos (numerod)
        ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE trabaja_en (
    nsse        CHAR(9)     NOT NULL,
    nump        INT     NOT NULL,
    horas       DECIMAL(3,1)    NOT NULL,
    PRIMARY KEY (nsse, nump),
    FOREIGN KEY (nsse) REFERENCES empleados (nss)
        ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (nump) REFERENCES proyectos (numerop)
        ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE dependientes (
    nsse        CHAR(9)     NOT NULL,
    nombre_dependiente  VARCHAR(30) NOT NULL,
    sexo        CHAR,
    fechan      DATE,
    parentesco  CHAR(30),
    PRIMARY KEY (nsse, nombre_dependiente),
    FOREIGN KEY (nsse) REFERENCES empleados (nss)
        ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO departamentos (nombred, numerod, nssgte, fechainicgte)
    VALUES ('Investigacion', 5, '333445555', '22-May-78'),
    ('Administracion', 4, '987654321', '01-Jan-85'),
    ('Direccion', 1, '888665555', '19-Jun-71');
   
INSERT INTO empleados (nombrep, inic, apellido, nss, fechan, dirección, sexo, salario, nsssuper, nd)
    VALUES ('Jose', 'B', 'Silva', '123456789', '09-Jan-55', 'Fresnos 731 Higueras MX', 'M', 30000, '333445555', 5),
    ('Federico', 'T', 'Vizcarra', '333445555', 'Dec-08-45', 'Valle 638 Higueras MX', 'M', 40000, '888665555', 5),
    ('Alicia', 'J', 'Zapata', '999887777', '19-Jul-58', 'Catillo 3321 Sucre MX', 'F', 25000, '987654321', 4),
    ('Jazmin', 'S', 'Valdes', '987654321', '20-Jan-31', 'Bravo 291 Belen MX', 'F', 43000, '888665555', 4),
    ('Ramon', 'k', 'Nieto', '666884444', '15-Sep-52', 'Espiga 875 Heras MX', 'M', 38000, '333445555', 5),
    ('Josefa', 'A', 'Esparza', '453453453', '31-Jul-62', 'Rosas 5631 Higueras MX', 'F', '25000', '333445555', 5),
    ('Ahmed', 'V', 'Jabbar', '98798798', '29-May-59', 'Dallas 980 Higueras MX', 'M', '25000', 987654321, 4),
    ('Jaime', 'E', 'Botello', '888665555', '10-Nov-27', 'Sorgo 450 Higueras MX', 'M', 55000, null, 1);

INSERT INTO lugares_deptos (numerod, lugard)
    VALUES (1, 'Higueras'),
    (4, 'Santiago'),
    (5, 'Belen'),
    (5, 'Sacramento'),
    (5, 'Higueras');

INSERT INTO proyectos (nombrepr, numerop, lugarp, numd)
    VALUES ('ProductoX', 1, 'Belen', 5),
    ('ProyectoY', 2, 'Sacramento', 5),
    ('ProyectoZ', 3, 'Higueras', 5),
    ('Automatizacion', 10, 'Santiago', 4),
    ('Reorganizacion', 20, 'Higueras', 1),
    ('Nuevaspresentaciones', 30, 'Santiago', 4);

INSERT INTO dependientes (nsse, nombre_dependiente, sexo, fechan, parentesco)
    VALUES ('333445555', 'Alicia', 'F', '05-Apr-76', 'Hija'),
    ('333445555', 'Teodoro', 'M', '25-Oct-73', 'Hijo'),
    ('333445555', 'Jobita', 'F', '03-May-48', 'Conyuge'),
    ('987654321', 'Abdiel', 'M', '29-Feb-32', 'Conyuge'),
    ('123456789', 'Miguel', 'M', '01-Jan-78', 'Hijo'),
    ('123456789', 'Alicia', 'F', '31-Dec-78', 'Hija'),
    ('123456789', 'Elizabeth', 'F', '05-May-57', 'Conyuge');

INSERT INTO trabaja_en (nsse, nump, horas)
    VALUES ('123456789', 1, 32.5),
    ('123456789', 2, 7.5),
    ('666884444', 3, 40.0),
    ('453453453', 1, 20.0),
    ('453453453', 2, 20.0),
    ('333445555', 2, 10.0),
    ('333445555', 3, 10.0),
    ('333445555', 10, 10.0),
    ('333445555', 20, 10.0),
    ('999887777', 30, 30.0),
    ('999887777', 10, 10.0),
    ('999887777', 20, 35.0),
    ('98798798', 30, 5.0),
    ('987654321', 30, 20.0),
    ('987654321', 20, 15.0),
    ('888665555', 20, 20.0);

ALTER TABLE departamentos ADD FOREIGN KEY (nssgte) REFERENCES empleados (nss)
        ON DELETE SET DEFAULT ON UPDATE CASCADE;

ALTER TABLE empleados ADD FOREIGN KEY (nsssuper) REFERENCES empleados (nss)
        ON DELETE SET NULL ON UPDATE CASCADE;
       
ALTER TABLE empleados ADD FOREIGN KEY (nd) REFERENCES departamentos (numerod)
        ON DELETE SET DEFAULT ON UPDATE CASCADE;
