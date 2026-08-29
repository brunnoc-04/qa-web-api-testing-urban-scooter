# 🛴 QA Web & API Testing — Urban Scooter

## 📖 Sobre o Projeto

Este projeto consiste na validação completa do sistema **Urban Scooter**, uma 
plataforma de aluguel de patinetes elétricos. O trabalho foi dividido em duas 
frentes: testes funcionais na interface web (frontend) e testes de API REST 
(backend), com o objetivo de garantir que ambos estivessem em conformidade com 
os requisitos técnicos documentados.

Foram desenvolvidos **157 casos de teste** no total, distribuídos entre validação 
de formulário web e endpoints de API. Desses, **56 bugs** foram identificados e 
reportados no Jira, com evidências e rastreabilidade completa. A maioria das 
falhas encontradas envolvia validação incorreta de campos, mensagens de erro 
divergentes dos requisitos e aceitação de entradas fora das regras especificadas.

## 🛠️ Ferramentas e Tecnologias

- **Postman** — testes de API REST (POST e DELETE)
- **Google Chrome (v85+)** — testes de frontend
- **Opera (v71+)** — testes de frontend (cross-browser)
- **Google Sheets** — documentação de casos de teste
- **Jira** — reporte e rastreamento de bugs
- **Resolução:** 1280x720 (ambos os navegadores)

## 📐 Técnicas Aplicadas

- **Partição de Equivalência** — divisão de entradas em classes válidas e inválidas
- **Análise de Valor Limite** — teste nos extremos dos intervalos aceitos
- **Teste Cross-Browser** — validação simultânea no Chrome e no Opera
- **Teste de Regressão** — verificação de funcionalidades após alterações

---

## 📊 Resumo dos Resultados

| Métrica | Valor |
|---------|-------|
| Total de casos de teste | 157 |
| Bugs encontrados | 56 |
| Campos de frontend testados | 9 |
| Endpoints de API testados | 2 |
| Navegadores testados | 2 (Chrome + Opera) |

---

## 🖥️ Parte 1: Testes de Frontend

O formulário "About Customer" do Urban Scooter foi validado em dois navegadores 
(Google Chrome v85+ e Opera v71+, ambos na resolução 1280x720). Todos os campos 
do formulário foram testados, aplicando partição de equivalência e análise de 
valor limite.

### Campos validados

| Campo | Tipo | Obrigatório | Regras de Validação |
|-------|------|-------------|---------------------|
| Nome | Campo de texto | ✅ Sim | 2 a 15 caracteres, apenas letras latinas, espaços e traços |
| Sobrenome | Campo de texto | ✅ Sim | 2 a 15 caracteres, apenas letras latinas, espaços e traços |
| Endereço | Campo de texto | ✅ Sim | 5 a 50 caracteres, letras, números, espaços, traços, pontos e vírgulas |
| Estação de metrô | Campo de texto com sugestão | ✅ Sim | Estações de metrô de Los Angeles (lista armazenada no backend) |
| Telefone | Campo de texto | ✅ Sim | 10 a 12 caracteres, somente números e sinal "+" obrigatório |
| Data de entrega | Lista suspensa do calendário | ✅ Sim | Somente datas a partir do dia seguinte (D+1) |
| Período de locação | Lista suspensa | ✅ Sim | 1 a 7 dias |
| Cor | Caixa de seleção | ➖ Não | Preto, cinza |
| Comentário | Campo de texto | ➖ Não | Máximo de 24 caracteres, letras, números, espaços, traços, pontos e vírgulas |

### Exemplos de Casos de Teste — Campo: Nome

| ID | Caso de teste | Dados de teste | Resultado esperado | Resultado real | Status | Bug |
|----|---------------|----------------|-------------------|----------------|--------|-----|
| 1 | Nome com 1 caractere (abaixo do limite) | "B" | Campo destacado em vermelho com mensagem "Insira um nome válido" | Campo destacado em vermelho com mensagem "Insira um nome válido" | Aprovado | — |
| 2 | Nome com 2 caracteres (limite mínimo) | "Br" | Aceito e permite avançar | Aceito e permite avançar | Aprovado | — |
| 3 | Nome com 15 caracteres (limite máximo) | "Brunno Cesar Nor" | Aceito e permite avançar | Aceito e permite avançar | Aprovado | — |
| 4 | Nome com 16 caracteres (acima do limite) | "Brunno Cesar Norr" | Campo destacado em vermelho com mensagem "Insira um nome válido" | Campo destacado em vermelho com mensagem "Insira um nome válido" | Aprovado | — |
| 5 | Nome apenas com espaços em branco | "   " | Campo destacado em vermelho com mensagem "Insira um nome válido" | Sistema aceita a entrada e permite avançar no fluxo | Reprovado | Bug reportado no Jira |

### Exemplos de Casos de Teste — Campo: Data de Entrega

| ID | Caso de teste | Dados de teste | Resultado esperado | Resultado real | Status | Bug |
|----|---------------|----------------|-------------------|----------------|--------|-----|
| 1 | Validar erro ao tentar selecionar a data atual (Hoje) | 13.08.2026 | Sistema não permite a seleção ou destaca o erro (apenas datas futuras) | Sistema permite a inclusão da data atual e nenhuma mensagem de erro é mostrada | Reprovado | CQT3-32 |
| 2 | Validar erro ao tentar selecionar uma data no passado | 04.07.2026 | Sistema não permite a seleção ou destaca o erro | Sistema permite a inclusão de data passada e nenhuma mensagem de erro é mostrada | Reprovado | CQT3-33 |
| 3 | Garantir aceitação da data do dia seguinte (Limite Mínimo) | 14.08.2026 | Data selecionada e aparece imediatamente no campo | Data selecionada e aparece imediatamente no campo | Aprovado | — |
| 4 | Garantir aceitação de uma data distante (Ex: Próximo mês) | 14.09.2026 | Data selecionada e aparece imediatamente no campo | Data selecionada e aparece imediatamente no campo | Aprovado | — |

---

## 🔌 Parte 2: Testes de Backend (API REST)

Os testes de API foram realizados via Postman, validando os seguintes endpoints:

- `POST /api/v1/courier` — validação dos campos `login`, `password` e `firstName`
- `DELETE /api/v1/courier/:id` — validação da exclusão de entregador pelo ID

### Parâmetros validados no POST /api/v1/courier

| Campo | Regras | Obrigatório |
|-------|--------|-------------|
| `login` | Apenas letras latinas, 2 a 10 caracteres | ✅ Sim |
| `firstName` | Apenas letras latinas, 2 a 10 caracteres | ➖ Não |
| `password` | Apenas números inteiros, exatamente 4 caracteres | ✅ Sim |

### Exemplos de Casos de Teste — Campo: login

| ID | Caso de teste | Dados de teste | Resultado esperado | Resultado real | Status | Bug |
|----|---------------|----------------|-------------------|----------------|--------|-----|
| 1 | Criar com login válido | `{"login": "Brunno", "password": "1234"}` | 201 Created | 201 Created | Aprovado | — |
| 2 | Retorno de erro ao utilizar login duplicado | `{"login": "Brunno", "password": "1234"}` | 409 Conflict | 409 Conflict | Aprovado | — |
| 3 | Retorno de erro ao utilizar caracteres não latinos | `{"login": "Брунно", "password": "1234"}` | 400 Bad Request | 201 Created | Reprovado | Bug reportado no Jira |
| 4 | Retorno de erro ao utilizar apenas números | `{"login": "123456", "password": "1234"}` | 400 Bad Request | 201 Created | Reprovado | Bug reportado no Jira |
| 5 | Retorno de erro ao utilizar caracteres especiais | `{"login": "Brun@no", "password": "1234"}` | 400 Bad Request | 201 Created | Reprovado | Bug reportado no Jira |

### Exemplos de Casos de Teste — Campo: password

| ID | Caso de teste | Dados de teste | Resultado esperado | Resultado real | Status | Bug |
|----|---------------|----------------|-------------------|----------------|--------|-----|
| 1 | Criar com password válida (exatamente 4 números) | `{"login": "ninja", "password": "1234", "firstName": "saske"}` | 201 Created | 201 Created | Aprovado | — |
| 2 | Retorno de erro abaixo do limite | `{"login": "ninjab", "password": "123", "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 201 Created | Reprovado | BCQT3-50 |
| 3 | Retorno de erro acima do limite | `{"login": "ninjac", "password": "12345", "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 201 Created | Reprovado | BCQT3-51 |
| 4 | Retorno de erro ao enviar o campo vazio | `{"login": "ninjad", "password": "", "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 400 Bad Request "Não há dados suficientes para criar uma conta" | Aprovado | — |
| 5 | Retorno de erro ao enviar valor nulo | `{"login": "ninjae", "password": null, "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 400 Bad Request "Não há dados suficientes para criar uma conta" | Aprovado | — |
| 6 | Retorno de erro ao omitir o parâmetro 'password' | `{"login": "ninjaf", "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 400 Bad Request "Não há dados suficientes para criar uma conta" | Aprovado | — |
| 7 | Retorno de erro ao utilizar letras latinas | `{"login": "ninjag", "password": "abcd", "firstName": "saske"}` | 400 Bad Request com mensagem "Não há dados suficientes para criar uma conta" | 201 Created | Reprovado | BCQT3-52 |

### Exemplos de Casos de Teste — Endpoint: DELETE /api/v1/courier/:id

| ID | Caso de teste | Dados de teste | Resultado esperado | Resultado real | Status | Bug |
|----|---------------|----------------|-------------------|----------------|--------|-----|
| 1 | Exclusão com ID válido | `{"id": "123"}` | 200 OK | 200 OK | Aprovado | — |
| 2 | Exclusão com ID vazio | `{"id": ""}` | 400 Bad Request com mensagem "Não há dados suficientes para remover o entregador" | 404 Not Found | Reprovado | Bug reportado no Jira |

---

## 🐛 Bugs Encontrados (exemplos)

### Bug 1 (Frontend): Campo "Nome" aceita apenas espaços em branco
- **Severidade:** Média
- **Ambiente:** Google Chrome e Opera (1280x720)
- **Requisito:** O campo deve aceitar apenas letras latinas, espaços e traços, com 2 a 15 caracteres úteis
- **Resultado esperado:** Exibir alerta "Insira um nome válido"
- **Resultado obtido:** Sistema aceita a entrada composta apenas por espaços e permite avançar no fluxo

### Bug 2 (Frontend): Campo "Data de Entrega" aceita data atual
- **Severidade:** Alta
- **Ambiente:** Google Chrome e Opera (1280x720)
- **Requisito:** Somente datas a partir do dia seguinte (D+1) podem ser escolhidas
- **Resultado esperado:** Sistema não permite a seleção da data atual
- **Resultado obtido:** Sistema permite a seleção da data atual sem exibir mensagem de erro
- **Jira:** CQT3-32

### Bug 3 (Frontend): Campo "Data de Entrega" aceita data no passado
- **Severidade:** Alta
- **Ambiente:** Google Chrome e Opera (1280x720)
- **Requisito:** Datas passadas não devem ser selecionáveis
- **Resultado esperado:** Sistema bloqueia a seleção
- **Resultado obtido:** Sistema permite a seleção de data passada sem exibir mensagem de erro
- **Jira:** CQT3-33

### Bug 4 (Backend): Campo `password` aceita 3 caracteres
- **Severidade:** Alta
- **Endpoint:** `POST /api/v1/courier`
- **Requisito:** O campo `password` deve conter exatamente 4 caracteres numéricos
- **Payload enviado:** `{"login": "ninjab", "password": "123", "firstName": "saske"}`
- **Resultado esperado:** `400 Bad Request` com mensagem "Não há dados suficientes para criar uma conta"
- **Resultado obtido:** `201 Created`
- **Jira:** BCQT3-50

### Bug 5 (Backend): Campo `password` aceita 5 caracteres
- **Severidade:** Alta
- **Endpoint:** `POST /api/v1/courier`
- **Requisito:** O campo `password` deve conter exatamente 4 caracteres numéricos
- **Payload enviado:** `{"login": "ninjac", "password": "12345", "firstName": "saske"}`
- **Resultado esperado:** `400 Bad Request`
- **Resultado obtido:** `201 Created`
- **Jira:** BCQT3-51

### Bug 6 (Backend): Campo `password` aceita letras latinas
- **Severidade:** Alta
- **Endpoint:** `POST /api/v1/courier`
- **Requisito:** O campo `password` deve aceitar apenas números
- **Payload enviado:** `{"login": "ninjag", "password": "abcd", "firstName": "saske"}`
- **Resultado esperado:** `400 Bad Request`
- **Resultado obtido:** `201 Created`
- **Jira:** BCQT3-52

### Bug 7 (Backend): Endpoint DELETE retorna erro incorreto com ID vazio
- **Severidade:** Média
- **Endpoint:** `DELETE /api/v1/courier/:id`
- **Requisito:** Quando o ID está vazio, deve retornar `400 Bad Request` com a mensagem "Não há dados suficientes para remover o entregador"
- **Resultado esperado:** `400 Bad Request`
- **Resultado obtido:** `404 Not Found`

---

## 📌 Sobre o Projeto

Este projeto foi fundamental pra consolidar a base de testes manuais. A principal 
dificuldade não estava em encontrar bugs óbvios, mas sim em identificar falhas 
onde o sistema "quase funcionava". Muitas das 56 inconsistências encontradas 
envolviam mensagens de erro com palavras diferentes das especificadas nos 
requisitos, campos que aceitavam um caractere a mais ou a menos do que o 
definido, e validações que deixavam passar entradas fora do padrão.

Um ponto crítico foi a descoberta de que o campo `password` aceitava tanto 3 
quanto 5 caracteres, quando o requisito é claro: exatamente 4 dígitos numéricos. 
Isso significa que a validação de limite não estava implementada corretamente no 
backend, o que representa uma falha de segurança relevante.

No frontend, o campo de data de entrega permitia selecionar tanto a data atual 
quanto datas passadas, quando a regra de negócio define que apenas datas a 
partir de D+1 são válidas. Esse tipo de falha pode impactar diretamente a 
operação do sistema de entregas.

A documentação completa dos 157 casos de teste foi feita no Google Sheets, com 
rastreabilidade direta para os bugs reportados no Jira. Cada bug contém screenshot 
de evidência, passos para reprodução, resultado esperado e resultado real.

---

## 🔗 Links e Documentação
- [Requisitos do Backend (PDF)](https://practicum-content.s3.us-west-1.amazonaws.com/new-markets/qa-final-project/PT/V8/backend_PT.pdf)
- [Requisitos do Aplicativo Web (PDF)](https://practicum-content.s3.us-west-1.amazonaws.com/new-markets/qa-final-project/PT/V8/Requisitos_do_aplicativo_web.pdf)
- [Planilha de casos de teste (Google Sheets)](https://docs.google.com/spreadsheets/d/1RqHsQNCkmFsTP95ERm_xJ1hYNlLkQ92Gl9PJo7J8fT4/edit?gid=0#gid=0)
