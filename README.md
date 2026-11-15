# 🏥 Plataforma de Gestão de Saúde — Clínica Vida+

Sistema integrado de gestão clínica e atendimento ambulatorial desenvolvido para solucionar os gargalos operacionais da **Clínica Vida+** (localizada na cidade de São Lucas), atendendo às demandas da diretoria administrativa (**Sra. Helena**), corpo clínico, recepção e pacientes.

---

## 📋 1. Contexto e Situação-Problema

Com o rápido crescimento da cidade de São Lucas, a **Clínica Vida+** — especializada em consultas médicas e exames laboratoriais — enfrentava dificuldades críticas em suas rotinas diárias:
* Agendamentos realizados de forma manual, gerando conflitos de horários e falhas de comunicação;
* Dificuldade dos médicos no acompanhamento ágil e centralizado do histórico de evolução clínica dos pacientes;
* Inconsistências recorrentes nas cobranças de consultas/exames e na consolidação de relatórios gerenciais mensais.

---

## 🎯 2. Objetivos e Requisitos do Sistema

### Requisitos Funcionais (RF)
* **RF01 — Gestão de Cadastros Base:** Cadastro, consulta, atualização e inativação de **Pacientes**, **Médicos** (com especialidade e CRM) e **Exames** laboratoriais/clínicos (com valores e preparos).
* **RF02 — Agendamento Inteligente:** Marcação de consultas e exames com controle em tempo real da grade de horários disponíveis e gestão de status (*Agendado, Confirmado, Realizado, Cancelado*).
* **RF03 — Registro de Atendimento e Prontuário:** Registro detalhado dos atendimentos realizados, contemplando sintomas, diagnósticos, prescrições e histórico cronológico de evolução clínica por paciente.
* **RF04 — Relatórios Administrativos:** Geração de relatórios mensais consolidados (volume de atendimentos, faturamento por especialidade/exame e produtividade) para apoio à tomada de decisão da administração.
* **RF05 — Autenticação e Controle de Acesso:** Sistema de login com níveis de permissão distintos (*Administração, Médico, Recepção e Paciente*).

### Requisitos Não Funcionais (RNF)
* **RNF01 — Segurança e Privacidade (LGPD):** Tráfego de dados criptografado em trânsito via protocolo **HTTPS (TLS/SSL)** e armazenamento seguro de senhas em repouso utilizando algoritmos de **Hash** (sem texto puro no banco de dados).
* **RNF02 — Disponibilidade e Integridade:** Persistência relacional com integridade referencial rigorosa (PK/FK) e rotinas de sincronização/backup em nuvem para recuperação rápida contra falhas.
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

* **`Paciente`**: Armazena dados cadastrais e de contato (`id_paciente`, `nome`, `cpf`, `data_nascimento`, `telefone`, `email`).
* **`Medico`**: Registra o corpo clínico (`id_medico`, `nome`, `crm`, `especialidade`, `telefone`, `email`).
* **`Exame`**: Catálogo de procedimentos laboratoriais e clínicos (`id_exame`, `nome_exame`, `descricao`, `custo`, `preparo`).
* **`Agendamento`**: Controla a agenda de consultas e exames (`id_agendamento`, `id_paciente`, `id_medico`, `id_exame`, `data_hora`, `tipo`, `status`).
* **`Atendimento`**: Registra a execução clínica e a evolução do paciente (`id_atendimento`, `id_agendamento`, `data_realizacao`, `sintomas`, `diagnostico`, `prescricao`).
* **`Usuario`**: Gerencia credenciais de acesso com hash de senha e perfis de permissão (`id_usuario`, `login`, `senha_hash`, `perfil`, `ativo`).

---

## 📁 5. Estrutura Inicial de Diretórios do Projeto

```text
plataforma-de-saude/
├── docs/                   # Diagramas (DER, UML) e documentações complementares
├── src/
│   ├── controllers/        # Rotas da API REST e controle de requisições HTTP/HTTPS
│   ├── models/             # Modelos de dados e mapeamento das entidades MySQL
│   ├── services/           # Regras de negócio, validações e geração de relatórios
│   ├── templates/          # Páginas HTML5 (Login, Dashboard, Agendamentos, Prontuário)
│   └── static/             # Arquivos CSS3, JavaScript e imagens
├── database/               # Scripts SQL (DDL de criação de tabelas e cargas iniciais)
├── .gitignore              # Exclusão de ambientes virtuais (venv), caches e logs
├── requirements.txt        # Dependências do ambiente Python
└── README.md               # Documentação principal do projeto
