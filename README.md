# 📊 DESAFIO DE PROJETO

## Integrando Dados com MySQL na Azure e Transformando com Power BI

---

## 🎯 Objetivo do Projeto

Este projeto tem como objetivo realizar um processo de ETL (Extração, Transformação e Carga) utilizando uma instância de banco de dados MySQL na Azure como fonte de dados e Power BI para tratamento e visualização, com foco na validação e análise de dados organizacionais.

---

## 🧩 Abordagem

Foi adotada uma abordagem **top-down**, iniciando pela análise e visualização dos dados, e posteriormente aprofundando nas transformações e modelagem.

---

## 🛠️ Tecnologias Utilizadas

* Azure Database for MySQL
* Power BI
* Power Query
* SQL

---

## ☁️ Ambiente em Nuvem (Azure)

Foi utilizada uma instância de banco de dados MySQL na Azure para simular um ambiente real de mercado, onde os dados são armazenados em cloud.

As principais etapas envolveram:

* Criação da instância de banco de dados na Azure
* Configuração de acesso ao banco
* Integração com o Power BI
* Consumo dos dados diretamente da nuvem

📌 *Os scripts completos de criação, inserção e consultas SQL estão disponíveis neste repositório.*

---

## 📥 Base de Dados

A base utilizada representa um cenário organizacional com informações de:

* Colaboradores (employee)
* Departamentos (departament)
* Projetos (project)
* Horas trabalhadas (works_on)
* Dependentes (dependent)
* Localizações (dept_locations)

---

# 🔄 ETL — Transformações Realizadas

## 🔹 1. Revisão de Tipos de Dados

Na tabela `employee`, foram revisados os tipos de dados para garantir consistência na análise:

* Salary como decimal
* Bdate como data
* Ssn e Super_ssn como texto

Esses ajustes foram necessários pois os campos representam identificadores e valores analíticos distintos.

---

## 🔹 2. Análise de Valores Nulos

Foi realizada a análise da coluna `Super_ssn`:

* O valor nulo foi mantido
* Representa colaboradores sem supervisor direto

Exemplo:
James Borg está no topo da hierarquia, portanto não possui gerente.

---

## 🔹 3. Verificação de Gerentes

* Colaboradores com `Super_ssn = NULL` podem ser gerentes
* Todos os departamentos possuem gerente

Na tabela `departament`, não foram encontrados valores nulos em `Mgr_ssn`.

---

## 🔹 4. Análise de Horas Trabalhadas

Na tabela `works_on`:

* Foi identificado registro com `Hours = 0`
* O valor foi mantido

Justificativa:

* O colaborador pode estar vinculado ao projeto sem registrar horas
* Para gerentes, esse comportamento é esperado

---

## 🔹 5. Tratamento de Colunas Complexas

A coluna `Address` foi transformada:

* Separada em Número, Rua, Cidade e Estado

Benefícios:

* Melhor legibilidade
* Facilita análises geográficas

---

## 🔹 6. Mesclagem de Tabelas (Employee + Department)

Foi realizada a mesclagem das tabelas:

* `employee` + `departament`
* Chave: `Dno = Dnumber`
* Tipo: Left Outer Join

Objetivo:

* Associar cada colaborador ao seu departamento

Motivo:

* As tabelas possuem informações complementares
* Não se aplica operação de append

---

## 🔹 7. Relacionamento Colaborador x Gerente (Self Join)

Foi realizada a mesclagem da tabela `employee` com ela mesma:

* Chave: `Super_ssn = Ssn`
* Tipo: Left Join

Objetivo:

* Identificar o gerente de cada colaborador

---

## 🔹 8. Padronização de Nomes

Foi realizada a concatenação dos nomes:

* Colaborador → Nome completo
* Gerente → Nome completo

Colunas renomeadas para:

* Colaborador
* Gerente

Objetivo:

* Melhorar legibilidade
* Tornar o modelo orientado ao negócio

---

## 🔹 9. Departamento + Localização

Foi realizada a mesclagem:

* `departament` + `dept_locations`

E criada uma coluna única:

* Departamento - Localização

Exemplo:

* Research - Houston

Objetivo:

* Criar chave única
* Evitar relacionamento muitos-para-muitos
* Preparar modelo estrela

Motivo de usar mesclagem:

* Dados complementares
* Necessidade de join
* Append não se aplica

---

## 🔹 10. Agrupamento de Dados

Foi criada uma nova consulta utilizando **Referenciar**:

* Agrupamento por Gerente
* Contagem de colaboradores

Objetivo:

* Analisar distribuição da equipe
* Identificar sobrecarga

---

## 🔹 11. Limpeza de Dados

* Remoção de colunas desnecessárias
* Padronização de nomes
* Ajuste de estrutura

---

# 📊 Relatório no Power BI

Foi desenvolvido um mini relatório com foco em validação e análise.

## Visualizações criadas:

* Tabela: Colaborador, Gerente e Departamento
* Gráfico: Colaboradores por Gerente
* Gráfico: Distribuição de Salários por Colaborador
* Gráfico: Salário por Gerente

---

## 🎯 Objetivo do Relatório

* Validar consistência dos dados
* Identificar relações hierárquicas
* Analisar distribuição da equipe
* Detectar possíveis inconsistências

---

# 📌 Observação

📎 Todos os scripts SQL utilizados (criação, inserção e consultas de validação) estão disponíveis neste repositório para consulta.

---

# 📈 Conclusão

Este projeto permitiu aplicar na prática:

* ETL com Power Query
* Integração com banco de dados em nuvem (Azure)
* Modelagem de dados
* Relacionamento entre tabelas
* Validação de dados
* Criação de relatórios no Power BI

Além disso, reforçou conceitos como:

* Uso de chaves para mesclagem
* Diferença entre Merge e Append
* Importância da análise de nulos
* Preparação para modelo estrela

