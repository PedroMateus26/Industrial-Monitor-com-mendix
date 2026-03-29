# 📊 Industrial Monitoring - Mendix Application

Sistema de monitoramento industrial desenvolvido em **Mendix**, com integração a API externa de temperatura, persistência em PostgreSQL e visualização em dashboard em tempo real.

---

## 🚀 Visão Geral

A aplicação realiza:

* Coleta de temperatura via API externa
* Cálculo de eficiência baseado em regra de negócio
* Persistência das leituras no banco de dados
* Atualização automática do dashboard
* Exibição de histórico e gráficos
* Tratamento de falha da API com fallback

---

## 🌐 Integração com API

A aplicação consome dados da API:

```text
https://api.openweathermap.org/data/2.5/weather
```

### 🔐 Configuração obrigatória

Para utilizar a API, é necessário:

1. Criar conta em: https://openweathermap.org/
2. Gerar uma **API Key**
3. Configurar no Mendix via constantes:

* `OWM_API_KEY`
* `OWM_LAT`
* `OWM_LON`

---

## 🧠 Regra de Negócio

```text
Eficiência = 23 + ((Temperatura - 21) * 7)
```

### Tratamentos aplicados:

* Valor mínimo: **23**
* Valor máximo: **100**
* Validação antes de persistir

---

## 🏗️ Arquitetura

### Fluxo geral:

```text
API → Microflows → Cálculo → Banco → Dashboard
```

---

## 🔄 Microflows

### 🔥 MF_CollectAndStoreReading (Core do sistema)

Responsável por:

1. Criar flag de erro (`ApiFailed`)
2. Chamar API (`MF_GetTemperatureFromApi`)
3. Validar temperatura
4. Calcular eficiência (`MF_CalculateEfficiency`)
5. Validar dados
6. Persistir no banco (SQL)
7. Tratar falha da API

👉 Possui fallback quando:

* API não responde
* Dados inválidos

---

### 🌐 MF_GetTemperatureFromApi

* Realiza chamada REST (GET)
* Mapeia resposta (`weather_response`)
* Retorna temperatura

👉 Integração externa isolada

---

### 🧠 MF_CalculateEfficiency

* Aplica fórmula de eficiência
* Garante limites (23 a 100)

👉 Regra de negócio isolada

---

### 🔄 MF_RefreshDashboard

Responsável por:

* Executar coleta de dados
* Buscar última leitura
* Buscar histórico
* Atualizar status do sistema

Status possíveis:

* OK
* ALERTA
* CRÍTICO

---

### 📊 MF_GetLatestReading

* Busca última leitura do banco
* Retorna objeto para UI

---

### 📈 MF_GetReadings

* Busca histórico (limitado)
* Alimenta gráficos e grid

---

## 📊 Dashboard

O dashboard apresenta:

* 🌡️ Temperatura atual
* ⚙️ Eficiência
* 🕒 Última leitura
* 📊 Histórico
* 📉 Gráficos de tendência

---

## ⚠️ Tratamento de Erros

* Uso de variável `ApiFailed`
* Fallback quando API falha
* Exibição de mensagem amigável na UI
* Continuidade do sistema mesmo sem API

---

## 🕒 Timezone

* Dados armazenados em **UTC**
* Conversão feita automaticamente no Mendix (`Localized = Yes`)

---

## 🛠️ Tecnologias Utilizadas

* Mendix
* PostgreSQL
* REST API
* Database Connector

---

## 📦 Como executar

1. Clonar o repositório
2. Configurar banco PostgreSQL
3. Criar tabelas (ver documentação)
4. Configurar constantes no Mendix
5. Executar aplicação

---

## 🧪 Boas práticas aplicadas

* Separação de responsabilidades (microflows)
* Tratamento de erro com fallback
* Armazenamento em UTC
* Validação antes de persistir
* Organização por camadas
* Integração desacoplada

---

## 🔮 Melhorias futuras

* Cache de dados
* WebSockets (real-time)
* Multi-máquinas
* Alertas push
* Paginação no histórico

---

## 👨‍💻 Autor

Projeto desenvolvido como desafio técnico com foco em:

* Integração de sistemas
* Modelagem de dados
* Arquitetura
* Boas práticas

---

## 🧠 Considerações finais

Este projeto demonstra:

* Integração com API externa
* Persistência estruturada
* Tratamento de falhas
* Atualização reativa de interface
* Organização arquitetural