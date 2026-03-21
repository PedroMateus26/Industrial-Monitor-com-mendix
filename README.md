Industrial Efficiency Monitor

📌 Descrição
Sistema para monitorar a eficiência de máquinas industriais com base na temperatura ambiente.

🧱 Banco de Dados
O sistema utiliza PostgreSQL.

Tabelas
machine_reading: armazena leituras de temperatura e eficiência
machine_alert: armazena alertas relacionados às leituras

⚙️ Como configurar o banco
Criar o banco:
CREATE DATABASE industrial_monitor;

Executar o script:
psql -U postgres -d industrial_monitor -f database/schema.sql