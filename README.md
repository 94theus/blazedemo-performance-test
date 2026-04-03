# 🚀 BlazeDemo Performance Test

## 📌 Descrição

Este projeto tem como objetivo validar a performance da aplicação **BlazeDemo** utilizando testes de carga e pico com Apache JMeter.

Foram simulados cenários reais de compra de passagem aérea, incluindo:

* Acesso à página inicial
* Busca de voos
* Seleção de voo
* Compra da passagem

---

## 🎯 Critério de Aceitação

* 250 requisições por segundo (RPS)
* Percentil 90 (P90) < 2 segundos

---

## 🛠️ Tecnologias utilizadas

* Apache JMeter
* Concurrency Thread Group (plugin)
* Throughput Shaping Timer (plugin)

---

## 📂 Estrutura do Projeto

```
📁 dashboards
 ┣ 📁 dashboard-load
 ┗ 📁 dashboard-spike

📁 jmx
 ┣ blazedemo-load-base.jmx
 ┗ blazedemo-spike-base.jmx

📄 resultado-load.jtl
📄 resultado-spike.jtl
```

---

## 🚀 Como executar os testes

### 🔹 Pré-requisitos

* Apache JMeter 5.6.3+
* Plugins instalados:

  * Concurrency Thread Group
  * Throughput Shaping Timer

---

### 🔹 Executar Teste de Carga

```bash
jmeter -n -t jmx/blazedemo-load-base.jmx -l resultado-load.jtl -e -o dashboards/dashboard-load
```

---

### 🔹 Executar Teste de Pico

```bash
jmeter -n -t jmx/blazedemo-spike-base.jmx -l resultado-spike.jtl -e -o dashboards/dashboard-spike
```

---

### ⚠️ Importante

Caso a pasta de dashboard já exista, remova antes de executar novamente:

```bash
rmdir /s /q dashboards\dashboard-load
rmdir /s /q dashboards\dashboard-spike
```

---

# 📊 Teste de Carga

## ⚙️ Configuração

* Usuários simultâneos: 500
* Ramp-up: 1 minuto
* Sustentação: 5 minutos
* Throughput: até 250 RPS

---

## ⏱️ Duração do teste

* Tempo total: ~6 minutos

---

## 📈 Resultados

* Throughput médio: ~226 req/s
* Percentil 90: ~5.4 segundos
* Taxa de erro: ~0.8%

---

## 🔎 Análise

Apesar do sistema suportar uma alta taxa de requisições, não foi possível manter os 250 RPS esperados, ficando abaixo da meta definida.

O tempo de resposta no percentil 90 ultrapassou significativamente o limite de 2 segundos, indicando degradação de performance sob carga sustentada.

Foram observados erros do tipo:

* Socket closed (possível instabilidade de conexão)
* HTTP 429 (Too Many Requests)

---

## 📌 Conclusão

❌ O critério de aceitação NÃO foi atendido.

O sistema não suportou a carga esperada mantendo os tempos de resposta dentro do limite estabelecido.

---

# ⚡ Teste de Pico

## ⚙️ Configuração

* Usuários simultâneos: 500
* Ramp-up: 1 minuto
* Pico de carga: até 400 RPS
* Sustentação final: 250 RPS

---

## ⏱️ Duração do teste

* Tempo total: ~4 minutos

---

## 📈 Resultados

* Throughput médio: ~231 req/s
* Percentil 90: ~4.2 segundos
* Taxa de erro: ~0.28%

---

## 🔎 Análise

O sistema apresentou boa capacidade de absorver picos repentinos de carga, mantendo uma taxa de erro relativamente baixa.

No entanto, o tempo de resposta degradou significativamente durante o pico e não retornou a níveis aceitáveis após estabilização.

Foram observados erros do tipo:

* HTTP 429 (Too Many Requests)

---

## 📌 Conclusão

❌ O critério de aceitação NÃO foi atendido.

Apesar de suportar picos de carga, o sistema não mantém o tempo de resposta dentro do limite de 2 segundos.

---

# 📊 Comparação entre Testes

| Métrica          | Carga      | Pico       |
| ---------------- | ---------- | ---------- |
| Throughput médio | ~226 req/s | ~231 req/s |
| Percentil 90     | ~5.4s      | ~4.2s      |
| Taxa de erro     | ~0.81%     | ~0.28%     |

---

## 🔎 Análise Comparativa

* O teste de carga apresentou maior degradação de performance ao longo do tempo
* O teste de pico demonstrou melhor controle de erros
* Em ambos os cenários, o tempo de resposta excedeu o limite definido

---

## 📌 Conclusão Final

❌ O sistema não atende ao critério de aceitação:

* Não sustenta 250 requisições por segundo de forma consistente
* Não mantém o tempo de resposta (P90 < 2s)

O comportamento observado indica limitação de capacidade sob carga elevada, possivelmente relacionada a:

* Limitação de infraestrutura
* Rate limiting (HTTP 429)
* Gargalos na aplicação

---

## 🧠 Consideração Final

Os resultados demonstram que a aplicação apresenta limitações claras sob alta carga e picos de acesso, evidenciando a necessidade de otimizações para suportar cenários reais de produção.

---

## ⚠️ Observação

O ambiente testado (https://www.blazedemo.com) é uma aplicação de demonstração pública e não foi projetado para suportar testes de alta carga, o que impacta diretamente nos resultados obtidos.
