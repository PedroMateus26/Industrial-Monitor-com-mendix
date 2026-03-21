🗄️ Configuração do Banco de Dados

Este projeto utiliza PostgreSQL como banco de dados para armazenar leituras de máquinas e alertas.

📌 Pré-requisitos
PostgreSQL instalado
Acesso ao terminal (psql) ou pgAdmin
🧱 1. Criar o banco de dados

Execute o comando abaixo:
CREATE DATABASE industrial_monitor;

Depois conecte ao banco:
\c industrial_monitor
ou selecione o banco pela própria ferramenta do pgadmin

📊 2. Criar tabela de leituras
CREATE TABLE machine_reading (
    id BIGSERIAL PRIMARY KEY,
    machine_id VARCHAR(50) NOT NULL,
    reading_time TIMESTAMP NOT NULL,
    temperature_c NUMERIC(5,2) NOT NULL,
    efficiency_pct NUMERIC(5,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

🔎 Descrição
machine_id: identificador da máquina
reading_time: momento da leitura
temperature_c: temperatura em graus Celsius
efficiency_pct: eficiência calculada
created_at: data de inserção no banco

🚨 3. Criar tabela de alertas
CREATE TABLE machine_alert (
    id BIGSERIAL PRIMARY KEY,
    machine_id VARCHAR(50) NOT NULL,
    reading_id BIGINT,
    alert_type VARCHAR(50) NOT NULL,
    severity VARCHAR(20) NOT NULL,
    message VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

🔗 4. Criar relacionamento (Foreign Key)
ALTER TABLE machine_alert
ADD CONSTRAINT fk_machine_alert_reading
FOREIGN KEY (reading_id) REFERENCES machine_reading(id);

🔎 O que isso faz
Garante que todo alerta esteja associado a uma leitura válida da tabela machine_reading.

🧪 5. Teste de inserção
Inserir leitura
INSERT INTO machine_reading (
    machine_id,
    reading_time,
    temperature_c,
    efficiency_pct
) VALUES (
    'MACHINE_01',
    NOW(),
    25.00,
    51.00
);

Inserir alerta
INSERT INTO machine_alert (
    machine_id,
    reading_id,
    alert_type,
    severity,
    message
) VALUES (
    'MACHINE_01',
    1,
    'LOW_EFFICIENCY',
    'HIGH',
    'Eficiência abaixo do esperado'
);

🔍 6. Consultar dados
Leituras
SELECT * FROM machine_reading;

Alertas
SELECT * FROM machine_alert;

Relacionamento entre tabelas
SELECT 
    r.id,
    r.temperature_c,
    r.efficiency_pct,
    a.alert_type,
    a.severity
FROM machine_reading r
LEFT JOIN machine_alert a ON a.reading_id = r.id;

⚠️ Observações
O banco não permite criar alertas com reading_id inexistente
Isso garante integridade dos dados
Utilize NOW() para timestamps atuais