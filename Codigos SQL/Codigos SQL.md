# CRIAÇÃO DE TABELAS


```sql
CREATE TABLE PACIENTE (
    ID_PACIENTE      NUMBER(10)      PRIMARY KEY,
    CPF              VARCHAR2(14)    NOT NULL UNIQUE,
    NOME             VARCHAR2(100)   NOT NULL,
    DATA_NASCIMENTO  DATE            NOT NULL,
    IDADE            NUMBER(3),
    TELEFONE         VARCHAR2(20),
    ENDERECO         VARCHAR2(200),
    PLANO_SAUDE      VARCHAR2(60)
);
```

```sql
CREATE TABLE PROFISSIONAL (
    ID_PROFISSIONAL   NUMBER(10)    PRIMARY KEY,
    NOME              VARCHAR2(100) NOT NULL,
    TIPO_PROFISSIONAL VARCHAR2(50)  NOT NULL,
    REGISTRO          VARCHAR2(30),
    ESPECIALIDADE     VARCHAR2(60),
    EMAIL             VARCHAR2(100)
);
```

```sql
CREATE TABLE SETOR (
    ID_SETOR     NUMBER(10)     PRIMARY KEY,
    NOME_SETOR   VARCHAR2(60)   NOT NULL,
    ANDAR        VARCHAR2(10)
);
```


```sql
CREATE TABLE LEITO (
    ID_LEITO       NUMBER(10)    PRIMARY KEY,
    CODIGO_LEITO   VARCHAR2(20)  NOT NULL,
    STATUS_LEITO   VARCHAR2(20)  NOT NULL,
    ID_SETOR       NUMBER(10)    NOT NULL,
    CONSTRAINT FK_LEITO_SETOR
        FOREIGN KEY (ID_SETOR)
        REFERENCES SETOR (ID_SETOR)
);
```

```sql
CREATE TABLE ATENDIMENTO (
    ID_ATENDIMENTO     NUMBER(10)    PRIMARY KEY,
    DATA_HORA          DATE          NOT NULL,
    TIPO_ATENDIMENTO   VARCHAR2(30)  NOT NULL,
    PRIORIDADE         VARCHAR2(20),
    DIAGNOSTICO        VARCHAR2(4000),
    VALOR_TOTAL        NUMBER(10,2),
    FORMA_PAGAMENTO    VARCHAR2(20),
    ID_PACIENTE        NUMBER(10)    NOT NULL,
    ID_PROFISSIONAL    NUMBER(10)    NOT NULL,
    ID_LEITO           NUMBER(10),

    CONSTRAINT FK_ATEND_PACIENTE
        FOREIGN KEY (ID_PACIENTE)
        REFERENCES PACIENTE (ID_PACIENTE),

    CONSTRAINT FK_ATEND_PROF
        FOREIGN KEY (ID_PROFISSIONAL)
        REFERENCES PROFISSIONAL (ID_PROFISSIONAL),

    CONSTRAINT FK_ATEND_LEITO
        FOREIGN KEY (ID_LEITO)
        REFERENCES LEITO (ID_LEITO)
);
```


# Inserir Dados


### SETOR

```sql
INSERT INTO SETOR (ID_SETOR, NOME_SETOR, ANDAR)
VALUES (1, 'EMERGENCIA', '1');

INSERT INTO SETOR (ID_SETOR, NOME_SETOR, ANDAR)
VALUES (2, 'INTERNACAO', '2');
```

### LEITO

```sql
INSERT INTO LEITO (ID_LEITO, CODIGO_LEITO, STATUS_LEITO, ID_SETOR)
VALUES (1, 'E101', 'LIVRE', 1);

INSERT INTO LEITO (ID_LEITO, CODIGO_LEITO, STATUS_LEITO, ID_SETOR)
VALUES (2, 'I201', 'OCUPADO', 2);
```

### PACIENTE

```sql
INSERT INTO PACIENTE (ID_PACIENTE, CPF, NOME, DATA_NASCIMENTO, IDADE,
                      TELEFONE, ENDERECO, PLANO_SAUDE)
VALUES (
    1,
    '111.111.111-11',
    'Ana Silva',
    TO_DATE('10/05/1990','DD/MM/YYYY'),
    34,
    '11999990000',
    'Rua A, 123 - Centro - São Paulo',
    'UNIMED'
);

INSERT INTO PACIENTE (ID_PACIENTE, CPF, NOME, DATA_NASCIMENTO, IDADE,
                      TELEFONE, ENDERECO, PLANO_SAUDE)
VALUES (
    2,
    '222.222.222-22',
    'Bruno Souza',
    TO_DATE('20/08/1985','DD/MM/YYYY'),
    39,
    '11988880000',
    'Rua B, 456 - Bairro B - São Paulo',
    'PARTICULAR'
);
```

### PROFISSIONAL

```sql
INSERT INTO PROFISSIONAL (ID_PROFISSIONAL, NOME, TIPO_PROFISSIONAL,
                          REGISTRO, ESPECIALIDADE, EMAIL)
VALUES (
    1,
    'Dr. Carlos',
    'MEDICO',
    'CRM12345',
    'CLINICO GERAL',
    'carlos@hospital.com'
);

INSERT INTO PROFISSIONAL (ID_PROFISSIONAL, NOME, TIPO_PROFISSIONAL,
                          REGISTRO, ESPECIALIDADE, EMAIL)
VALUES (
    2,
    'Enf. Marina',
    'ENFERMEIRO',
    'COREN54321',
    'ENFERMAGEM',
    'marina@hospital.com'
);
```

### ATENDIMENTO

```sql
-- Consulta na emergência
INSERT INTO ATENDIMENTO (ID_ATENDIMENTO, DATA_HORA, TIPO_ATENDIMENTO,
                         PRIORIDADE, DIAGNOSTICO, VALOR_TOTAL,
                         FORMA_PAGAMENTO, ID_PACIENTE, ID_PROFISSIONAL,
                         ID_LEITO)
VALUES (
    1,
    TO_DATE('25/11/2025 14:30','DD/MM/YYYY HH24:MI'),
    'CONSULTA',
    'URGENTE',
    'Dor torácica',
    300.00,
    'CONVENIO',
    1,
    1,
    NULL
);

-- Internação com leito
INSERT INTO ATENDIMENTO (ID_ATENDIMENTO, DATA_HORA, TIPO_ATENDIMENTO,
                         PRIORIDADE, DIAGNOSTICO, VALOR_TOTAL,
                         FORMA_PAGAMENTO, ID_PACIENTE, ID_PROFISSIONAL,
                         ID_LEITO)
VALUES (
    2,
    TO_DATE('26/11/2025 09:00','DD/MM/YYYY HH24:MI'),
    'INTERNACAO',
    'NORMAL',
    'Pós-operatório',
    1500.00,
    'PARTICULAR',
    2,
    1,
    2
);

COMMIT;
```


# Consultas SQL


### 1- Listar todos os pacientes

```sql
SELECT *
FROM PACIENTE;
```


### 2- Listar todos os atendimentos com nome do paciente e do profissional

```sql
SELECT
    a.ID_ATENDIMENTO,
    a.DATA_HORA,
    a.TIPO_ATENDIMENTO,
    a.PRIORIDADE,
    p.NOME  AS NOME_PACIENTE,
    pr.NOME AS NOME_PROFISSIONAL
FROM ATENDIMENTO a
JOIN PACIENTE p
    ON a.ID_PACIENTE = p.ID_PACIENTE
JOIN PROFISSIONAL pr
    ON a.ID_PROFISSIONAL = pr.ID_PROFISSIONAL;

```


### 3- Listar leitos livres

```sql
SELECT *
FROM LEITO
WHERE UPPER(STATUS_LEITO) = 'LIVRE';
```


### 4- Quantidade de atendimentos por profissional

```sql
SELECT
    pr.NOME AS NOME_PROFISSIONAL,
    COUNT(*) AS QTDE_ATENDIMENTOS
FROM ATENDIMENTO a
JOIN PROFISSIONAL pr
    ON a.ID_PROFISSIONAL = pr.ID_PROFISSIONAL
GROUP BY pr.NOME
ORDER BY QTDE_ATENDIMENTOS DESC;
```


### 5- Quantidade de atendimentos por setor (usando o setor do leito)

```sql
SELECT
    s.NOME_SETOR,
    COUNT(a.ID_ATENDIMENTO) AS QTDE_ATENDIMENTOS
FROM ATENDIMENTO a
LEFT JOIN LEITO l
    ON a.ID_LEITO = l.ID_LEITO
LEFT JOIN SETOR s
    ON l.ID_SETOR = s.ID_SETOR
GROUP BY s.NOME_SETOR;
```


### 6- Taxa de ocupação de leitos (relatório gerencial)

```sql
SELECT
    COUNT(*) AS TOTAL_LEITOS,
    SUM(CASE WHEN UPPER(STATUS_LEITO) = 'OCUPADO' THEN 1 ELSE 0 END) AS LEITOS_OCUPADOS,
    ROUND(
        (SUM(CASE WHEN UPPER(STATUS_LEITO) = 'OCUPADO' THEN 1 ELSE 0 END) / COUNT(*)) * 100,
        2
    ) AS TAXA_OCUPACAO_PCT
FROM LEITO;
```