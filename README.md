# 🏥 Plataforma de Gestão de Saúde — Clínica Vida+

Sistema integrado de gestão clínica e atendimento ambulatorial desenvolvido para solucionar os gargalos operacionais da **Clínica Vida+**, atendendo às demandas da diretoria administrativa (**Sra. Helena**), corpo clínico, recepção e pacientes.

---

## 📋 1. Contexto e Situação-Problema

Com o rápido crescimento da cidade, a **Clínica Vida+** — especializada em consultas médicas e exames laboratoriais — enfrentava dificuldades críticas em suas rotinas diárias:
* Agendamentos realizados de forma manual, gerando conflitos de horários e falhas de comunicação;
* Dificuldade dos médicos no acompanhamento ágil e centralizado do histórico de evolução clínica dos pacientes;
* Inconsistências recorrentes nas cobranças de consultas, aplicação de descontos por convênios/planos de saúde, controle de exames complementares realizáveis na clínica e consolidação de relatórios gerenciais mensais.

---

## 🎯 2. Objetivos e Requisitos do Sistema

### Requisitos Funcionais (RF)
* **RF01 — Gestão de Cadastros Base:** Cadastro, consulta, atualização e inativação de **Pacientes** (armazenando a `data_nascimento` para cálculo dinâmico de idade, perfil demográfico e relatórios gerenciais), **Médicos** (com especialidade e CRM), **Convênios/Planos de Saúde** e **Exames** laboratoriais/clínicos.
* **RF02 — Agendamento Inteligente:** Marcação de consultas e exames com controle em tempo real da grade de horários disponíveis e gestão de status (*Agendado, Confirmado, Realizado, Cancelado*).
* **RF03 — Registro de Atendimento, Convênios e Cobrança Dinâmica:** Registro detalhado dos atendimentos, contemplando sintomas, diagnósticos, prescrições, aplicação automática de regras de convênio (ex: desconto em consultas de R$ 600,00 para R$ 530,00) e acréscimo de valores referentes a exames complementares realizados dentro da clínica.
* **RF04 — Relatórios Administrativos e Estatísticas:** Geração de relatórios mensais consolidados (volume de atendimentos, faturamento por especialidade/exame, estatísticas de idade de pacientes e produtividade financeira) para apoio à tomada de decisão da administração.
* **RF05 — Autenticação e Controle de Acesso:** Sistema de login com níveis de permissão distintos (*Administração, Médico, Recepção e Paciente*).

### Requisitos Não Funcionais (RNF)
* **RNF01 — Segurança e Privacidade (LGPD):** Tráfego de dados criptografado em trânsito via protocolo **HTTPS (TLS/SSL)** e armazenamento seguro de senhas em repouso utilizando algoritmos de **Hash** (sem texto puro no banco de dados).
* **RNF02 — Disponibilidade, Integridade e Resiliência:** Persistência relacional com integridade referencial rigorosa (PK/FK), tratamento de transações em quedas de energia e rotinas de sincronização/backup em nuvem para recuperação rápida contra desastres.
* **RNF03 — Usabilidade e Multiplataforma:** Interface intuitiva para operação rápida no posto de atendimento (Desktop/Web) e acesso responsivo para pacientes (Mobile/Web).

---

## 🛠️ 3. Arquitetura e Tecnologias

* **Linguagem Backend & API:** Python (com integração via **API RESTful** sobre protocolo **HTTPS** para comunicação segura entre os ambientes Desktop e Mobile/Web)[cite: 1].
* **Plataforma de Execução:** Aplicação Desktop/Web (posto de atendimento e administração da clínica) + Acesso Mobile/Web para pacientes[cite: 1].
* **Tecnologia Frontend / Interface:** HTML5, CSS3, JavaScript e Templates[cite: 1].
* **Padrão de Projeto:** Arquitetura em Camadas / **MVC** (*Modelos, Views/Controllers e Serviços*)[cite: 1].
* **Segurança & Autenticação:** Controle de sessão por perfil de usuário, comunicação criptografada via **HTTPS** e proteção de credenciais com **Hash** (conformidade com a LGPD).
* **Banco de Dados & Nuvem:** Banco de dados relacional (SQL) com **XAMPP/MySQL** em ambiente de desenvolvimento e sincronização/backup em Nuvem para redundância e recuperação de desastres[cite: 1].
* **Ambiente de Desenvolvimento:** Visual Studio Code (IDE)[cite: 1].
* **Gestão Ágil e Versionamento:** Trello (Kanban/Scrum) + Git & GitHub[cite: 1].

---

## 🗄️ 4. Modelagem do Banco de Dados (DER)

A estrutura relacional foi projetada antes da implementação física no SGBD (**XAMPP/MySQL**), garantindo integridade referencial através de chaves primárias (`PK`) e estrangeiras (`FK`):

* **`Convenio`**: Gerencia os planos de saúde parceiros e regras de desconto aplicáveis (`id_convenio`, `nome_convenio`, `percentual_desconto`, `valor_fixo_consulta`).
* **`Paciente`**: Armazena dados cadastrais, de contato e data de nascimento para cálculo dinâmico de idade via SQL/Python (`id_paciente`, `id_convenio`, `nome`, `cpf`, `data_nascimento`, `telefone`, `email`).
* **`Medico`**: Registra o corpo clínico (`id_medico`, `nome`, `crm`, `especialidade`, `telefone`, `email`).
* **`Exame`**: Catálogo de procedimentos laboratoriais e clínicos realizados internamente (`id_exame`, `nome_exame`, `descricao`, `custo`, `preparo`).
* **`Agendamento`**: Controla a agenda de consultas e exames (`id_agendamento`, `id_paciente`, `id_medico`, `data_hora`, `tipo`, `status`).
* **`Atendimento`**: Registra a execução clínica, evolução, exames adicionados e composição financeira final da cobrança (`id_atendimento`, `id_agendamento`, `data_realizacao`, `sintomas`, `diagnostico`, `prescricao`, `valor_final`).
* **`Usuario`**: Gerencia credenciais de acesso com hash de senha e perfis de permissão (`id_usuario`, `login`, `senha_hash`, `perfil`, `ativo`).

---

## 📁 5. Estrutura Inicial de Diretórios do Projeto

```text
plataforma-de-saude/
├── docs/                   # Diagramas (DER, UML) e documentações complementares
├── src/
│   ├── controllers/        # Rotas da API REST e controle de requisições HTTP/HTTPS
│   ├── models/             # Modelos de dados e mapeamento das entidades MySQL
│   ├── services/           # Regras de negócio (cálculo de convênios, exames e relatórios)
│   ├── templates/          # Páginas HTML5 (Login, Dashboard, Agendamentos, Prontuário)
│   └── static/             # Arquivos CSS3, JavaScript e imagens
├── database/               # Scripts SQL (DDL de criação de tabelas e cargas iniciais)
├── .gitignore              # Exclusão de ambientes virtuais (venv), caches e logs
├── requirements.txt        # Dependências do ambiente Python
└── README.md               # Documentação principal do projeto
