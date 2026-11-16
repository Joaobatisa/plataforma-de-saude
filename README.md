# 🏥 Plataforma de Gestão de Saúde — Clínica Vida+

Sistema integrado de gestão clínica, controle de acesso e atendimento ambulatorial desenvolvido para solucionar os gargalos operacionais da **Clínica Vida+** (modelo de atendimento particular e convênios), atendendo às demandas da diretoria administrativa (**Sra. Helena**), corpo clínico, recepção e pacientes.

---

## 📋 1. Contexto e Situação-Problema

Com o rápido crescimento da cidade, a **Clínica Vida+** — especializada em consultas médicas e exames laboratoriais — enfrentava dificuldades críticas em suas rotinas diárias:
* Agendamentos realizados de forma manual, sem visibilidade clara da grade de dias, horários e salas de atendimento dos médicos especialistas;
* Dificuldade dos médicos no acompanhamento ágil e centralizado do histórico clínico e status de adimplência dos pacientes;
* Inconsistências nas cobranças de consultas, aplicação de descontos por convênios/planos de saúde, controle de exames complementares realizados na clínica e consolidação de relatórios gerenciais.

---

## 🎯 2. Objetivos e Requisitos do Sistema

### Requisitos Funcionais (RF)
* **RF01 — Gestão de Cadastros Base:** Cadastro, consulta, atualização e inativação de **Pacientes** (armazenando a `data_nascimento` para cálculo dinâmico de idade e perfil demográfico), **Médicos** (com CRM, especialidade, sala de atendimento e grade de dias/horários disponíveis), **Convênios/Planos de Saúde** e **Exames** laboratoriais/clínicos.
* **RF02 — Agendamento Inteligente e Disponibilidade Médica:** Consulta de disponibilidade por especialidade, dia e horário do médico, com indicação da **sala de atendimento** ao paciente e gestão de status (*Agendado, Confirmado, Realizado, Cancelado*).
* **RF03 — Controle de Acesso Automático (Lógica Booleana):** Validação automatizada de elegibilidade antes da liberação do atendimento na recepção, avaliando as variáveis lógicas **A** (*agendamento marcado*), **B** (*documentos RG/CPF válidos*), **C** (*médico disponível no dia/horário*) e **D** (*pagamentos anteriores em dia*):
  * **Consulta Normal:** Liberada se `(A ∧ B ∧ C) ∨ (B ∧ C ∧ D)` — *(Tem agendamento E documentos OK E médico disponível) OU (Documentos OK E médico disponível E pagamentos em dia)*.
  * **Atendimento de Emergência:** Liberado se `C ∧ (B ∨ D)` — *(Há médico disponível) E (Tem documentos OK OU pagamentos em dia)*.
* **RF04 — Registro de Atendimento, Convênios e Faturamento:** Registro clínico de evolução, aplicação automática de regras de convênio (ex: desconto na consulta base de R$ 600,00 para R$ 530,00 para conveniados adimplentes) e acréscimo de valores referentes a exames complementares realizados internamente.
* **RF05 — Comunicação e Notificações:** Envio de confirmações de agendamento, lembretes de horário/sala e orientações de preparo de exames via **E-mail** (backend Python) e **WhatsApp** (link direto integrado).
* **RF06 — Relatórios Administrativos e Estatísticas:** Geração de indicadores gerenciais (total de pacientes, idade média, identificação do paciente mais novo e mais velho, busca nominal e faturamento mensal por especialidade/convênio).
* **RF07 — Autenticação e Controle de Acesso por Perfil:** Sistema de login com níveis de permissão distintos (*Administração, Médico, Recepção e Paciente*).

### Requisitos Não Funcionais (RNF)
* **RNF01 — Segurança e Privacidade (LGPD):** Tráfego de dados criptografado em trânsito via protocolo **HTTPS (TLS/SSL)** e armazenamento seguro de senhas em repouso utilizando algoritmos de **Hash** (sem texto puro no banco de dados).
* **RNF02 — Disponibilidade, Integridade e Resiliência:** Persistência relacional com integridade referencial rigorosa (PK/FK), tratamento de transações ACID contra falhas de energia e rotinas de sincronização/backup em nuvem para recuperação contra desastres.
* **RNF03 — Usabilidade e Multiplataforma:** Interface intuitiva para operação rápida no posto de atendimento (Desktop/Web), acesso responsivo para pacientes (Mobile/Web) e módulo protótipo CLI em Python para rotinas de triagem e estatísticas.

---

## 🛠️ 3. Arquitetura e Tecnologias

* **Linguagem Backend & API:** Python (com integração via **API RESTful** sobre protocolo **HTTPS** para comunicação segura entre os ambientes Desktop e Mobile/Web, além de módulo CLI em Python para prototipagem e controle lógico).
* **Plataforma de Execução:** Aplicação Desktop/Web (posto de atendimento e administração da clínica) + Acesso Mobile/Web para pacientes.
* **Tecnologia Frontend / Interface:** HTML5, CSS3, JavaScript e Templates.
* **Padrão de Projeto:** Arquitetura em Camadas / **MVC** (*Modelos, Views/Controllers e Serviços*).
* **Segurança & Autenticação:** Controle de sessão por perfil de usuário, comunicação criptografada via **HTTPS** e proteção de credenciais com **Hash** (conformidade com a LGPD).
* **Banco de Dados & Nuvem:** Banco de dados relacional (SQL) com **XAMPP/MySQL** em ambiente de desenvolvimento e sincronização/backup em Nuvem para redundância e recuperação de desastres.
* **Comunicação Externa:** Disparo automatizado de E-mails (`smtplib` nativo Python) e integração de mensagens via link direto WhatsApp (`wa.me`).
* **Ambiente de Desenvolvimento:** Visual Studio Code (IDE).
* **Gestão Ágil e Versionamento:** Trello (Kanban/Scrum) + Git & GitHub.

---

## 🗄️ 4. Modelagem do Banco de Dados (DER)

A estrutura relacional foi projetada antes da implementação física no SGBD (**XAMPP/MySQL**), garantindo integridade referencial através de chaves primárias (`PK`) e estrangeiras (`FK`):

* **`Convenio`**: Gerencia os planos de saúde parceiros e regras de desconto aplicáveis (`id_convenio`, `nome_convenio`, `percentual_desconto`, `valor_fixo_consulta`).
* **`Paciente`**: Armazena dados cadastrais, status de adimplência e data de nascimento para cálculo dinâmico de idade (`id_paciente`, `id_convenio`, `nome`, `cpf`, `data_nascimento`, `telefone_whatsapp`, `email`, `documentos_validos`, `pagamentos_em_dia`).
* **`Medico`**: Registra o corpo clínico, especialidade e alocação física (`id_medico`, `nome`, `crm`, `especialidade`, `sala_atendimento`, `telefone`, `email`).
* **`Agenda_Medico`**: Controla os dias da semana, turnos e horários disponíveis de cada médico para exibição ao cliente (`id_agenda`, `id_medico`, `dia_semana`, `hora_inicio`, `hora_fim`, `disponivel`).
* **`Exame`**: Catálogo de procedimentos laboratoriais e clínicos realizados internamente (`id_exame`, `nome_exame`, `descricao`, `custo`, `preparo`).
* **`Agendamento`**: Controla a marcação de consultas e exames vinculados ao médico, horário e sala (`id_agendamento`, `id_paciente`, `id_medico`, `data_hora`, `sala`, `tipo`, `status`).
* **`Atendimento`**: Registra a execução clínica, evolução, exames adicionados e composição financeira final da cobrança (`id_atendimento`, `id_agendamento`, `data_realizacao`, `sintomas`, `diagnostico`, `prescricao`, `valor_final`).
* **`Usuario`**: Gerencia credenciais de acesso com hash de senha e perfis de permissão (`id_usuario`, `login`, `senha_hash`, `perfil`, `ativo`).

---

## 📁 5. Estrutura Inicial de Diretórios do Projeto

```text
plataforma-de-saude/
├── docs/                   # Diagramas (DER, UML, Tabela-Verdade) e documentações
├── prototype_cli/          # Módulo Python CLI (Menu interativo, estatísticas e lógica booleana)
├── src/
│   ├── controllers/        # Rotas da API REST e controle de requisições HTTP/HTTPS
│   ├── models/             # Modelos de dados e mapeamento das entidades MySQL
│   ├── services/           # Regras de negócio (convênios, exames, lógica de acesso e e-mail)
│   ├── templates/          # Páginas HTML5 (Login, Dashboard, Agendamentos, Prontuário)
│   └── static/             # Arquivos CSS3, JavaScript e imagens
├── database/               # Scripts SQL (DDL de criação de tabelas e cargas iniciais)
├── .gitignore              # Exclusão de ambientes virtuais (venv), caches e logs
├── requirements.txt        # Dependências do ambiente Python
└── README.md               # Documentação principal do projeto

