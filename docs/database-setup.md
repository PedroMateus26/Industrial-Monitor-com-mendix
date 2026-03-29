🧱 1. Criar o banco de dados
CREATE DATABASE industrial_monitor;

Conectar ao banco:

\c industrial_monitor
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
reading_time: momento da leitura (armazenado em UTC)
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
🔎 O que isso garante
Integridade referencial
Um alerta sempre está vinculado a uma leitura válida
🧪 5. Testes de inserção
Inserir leitura
INSERT INTO machine_reading (
    machine_id,
    reading_time,
    temperature_c,
    efficiency_pct
) VALUES (
    'MACHINE_01',
    CURRENT_TIMESTAMP,
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
🔍 6. Consultas úteis
Todas as leituras
SELECT * FROM machine_reading;
Todos os alertas
SELECT * FROM machine_alert;
Join entre leituras e alertas
SELECT 
    r.id,
    r.temperature_c,
    r.efficiency_pct,
    a.alert_type,
    a.severity
FROM machine_reading r
LEFT JOIN machine_alert a 
    ON a.reading_id = r.id;
⚠️ Observações importantes
Os timestamps são armazenados em UTC
A conversão para horário local é feita na aplicação (Mendix)
Não é possível inserir alertas com reading_id inexistente
Utilize CURRENT_TIMESTAMP para registros atuais
🧠 Boas práticas aplicadas
Uso de BIGSERIAL para escalabilidade
Separação entre leituras e alertas
Integridade referencial com Foreign Key
Armazenamento em UTC
Estrutura preparada para expansão futura