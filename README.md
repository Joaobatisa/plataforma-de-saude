# 🏥 Plataforma de Gestão de Saúde — Clínica Vida+

Sistema integrado de gestão clínica, controle de acesso e atendimento ambulatorial desenvolvido para solucionar os gargalos operacionais da **Clínica Vida+** (modelo de atendimento particular e convênios), atendendo às demandas da diretoria administrativa (Sra. Helena), corpo clínico, recepção e pacientes.

---

## 📋 1. Contexto e Situação-Problema
Com o rápido crescimento da cidade de São Lucas, a **Clínica Vida+** — especializada em consultas médicas e exames laboratoriais — enfrentava dificuldades críticas em suas rotinas diárias:
- Agendamentos realizados de forma manual, sem visibilidade clara da grade de dias, horários e salas de atendimento dos médicos especialistas;
- Dificuldade dos médicos no acompanhamento ágil e centralizado do histórico clínico e status de adimplência dos pacientes;
- Inconsistências nas cobranças de consultas, aplicação de descontos por convênios/planos de saúde, controle de exames complementares realizados na clínica e consolidação de relatórios gerenciais.

---

## 🎯 2. Objetivos e Requisitos do Sistema

### Requisitos Funcionais (RF)
- **RF01 — Gestão de Cadastros Base:** Cadastro, consulta, atualização e inativação de Pacientes (armazenando a `data_nascimento` para cálculo dinâmico de idade e perfil demográfico), Médicos (com CRM, especialidade, sala de atendimento e grade de dias/horários disponíveis), Convênios/Planos de Saúde e Exames laboratoriais/clínicos.
- **RF02 — Agendamento Inteligente e Disponibilidade Médica:** Consulta de disponibilidade por especialidade, dia e horário do médico, com indicação da sala de atendimento ao paciente e gestão de status (*Agendado, Confirmado, Realizado, Cancelado*).
- **RF03 — Controle de Acesso Automático (Lógica Booleana):** Validação automatizada de elegibilidade antes da liberação do atendimento na recepção, avaliando as variáveis lógicas $A$ (agendamento marcado), $B$ (documentos RG/CPF válidos), $C$ (médico disponível no dia/horário) e $D$ (pagamentos anteriores em dia):
  - **Consulta Normal:** Liberada se $(A \land B \land C) \lor (B \land C \land D)$ — *(Forma otimizada: $B \land C \land (A \lor D)$)*.
  - **Atendimento de Emergência:** Liberado se $C \land (B \or D)$.
- **RF04 — Registro de Atendimento, Convênios e Faturamento:** Registro clínico de evolução, aplicação automática de regras de convênio (ex: desconto na consulta base de R$ 600,00 para R$ 530,00 para conveniados adimplentes) e acréscimo de valores referentes a exames complementares realizados internamente.
- **RF05 — Comunicação e Notificações:** Envio de confirmações de agendamento, lembretes de horário/sala e orientações de preparo de exames via E-mail (backend Python) e WhatsApp (link direto integrado).
- **RF06 — Relatórios Administrativos e Estatísticas:** Geração de indicadores gerenciais (total de pacientes, idade média, identificação do paciente mais novo e mais velho, busca nominal e faturamento mensal por especialidade/convênio).
- **RF07 — Autenticação e Controle de Acesso por Perfil:** Sistema de login com níveis de permissão distintos (*Administração, Médico, Recepção e Paciente*).

### Requisitos Não Funcionais (RNF)
- **RNF01 — Segurança e Privacidade (LGPD):** Tráfego de dados criptografado em trânsito via protocolo HTTPS (TLS/SSL) e armazenamento seguro de senhas em repouso utilizando algoritmos de Hash (sem texto puro no banco de dados).
- **RNF02 — Disponibilidade, Integridade e Resiliência:** Persistência relacional com integridade referencial rigorosa (PK/FK), tratamento de transações ACID contra falhas de energia e rotinas de sincronização/backup em nuvem para recuperação contra desastres.
- **RNF03 — Usabilidade e Multiplataforma:** Interface intuitiva para operação rápida no posto de atendimento (Desktop/Web), acesso responsivo para pacientes (Mobile/Web) e módulo protótipo CLI em Python para rotinas de triagem e estatísticas.

---

## 🛠️ 3. Arquitetura e Tecnologias
- **Linguagem Backend & API:** Python (com integração via API RESTful sobre protocolo HTTPS para comunicação segura entre os ambientes Desktop e Mobile/Web, além de módulo CLI em Python para prototipagem e controle lógico).
- **Plataforma de Execução:** Aplicação Desktop/Web (posto de atendimento e administração da clínica) + Acesso Mobile/Web para pacientes.
- **Tecnologia Frontend / Interface:** HTML5, CSS3, JavaScript e Templates.
- **Padrão de Projeto:** Arquitetura em Camadas / MVC (Modelos, Views/Controllers e Serviços).
- **Segurança & Autenticação:** Controle de sessão por perfil de usuário, comunicação criptografada via HTTPS e proteção de credenciais com Hash (conformidade com a LGPD).
- **Banco de Dados & Nuvem:** Banco de dados relacional (SQL) com XAMPP/MySQL em ambiente de desenvolvimento e sincronização/backup em Nuvem para redundância e recuperação de desastres.
- **Comunicação Externa:** Disparo automatizado de E-mails (`smtplib` nativo Python) e integração de mensagens via link direto WhatsApp (`wa.me`).
- **Ambiente de Desenvolvimento:** Visual Studio Code (IDE).
- **Gestão Ágil e Versionamento:** Trello (Kanban/Scrum) + Git & GitHub.

---

## 🗄️ 4. Modelagem do Banco de Dados (DER)
A estrutura relacional foi projetada garantindo integridade referencial através de chaves primárias (PK) e estrangeiras (FK):
- `Convenio`: Planos de saúde parceiros e regras de desconto (`id_convenio`, `nome_convenio`, `percentual_desconto`, `valor_fixo_consulta`).
- `Paciente`: Dados cadastrais, adimplência e data de nascimento para cálculo de idade (`id_paciente`, `id_convenio`, `nome`, `cpf`, `data_nascimento`, `telefone_whatsapp`, `email`, `documentos_validos`, `pagamentos_em_dia`).
- `Medico`: Corpo clínico, especialidade e alocação (`id_medico`, `nome`, `crm`, `especialidade`, `sala_atendimento`, `telefone`, `email`).
- `Agenda_Medico`: Dias, turnos e horários disponíveis (`id_agenda`, `id_medico`, `dia_semana`, `hora_inicio`, `hora_fim`, `disponivel`).
- `Exame`: Procedimentos laboratoriais e clínicos (`id_exame`, `nome_exame`, `descricao`, `custo`, `preparo`).
- `Agendamento`: Marcação de consultas e exames (`id_agendamento`, `id_paciente`, `id_medico`, `data_hora`, `sala`, `tipo`, `status`).
- `Atendimento`: Execução clínica, evolução e composição financeira (`id_atendimento`, `id_agendamento`, `data_realizacao`, `sintomas`, `diagnostico`, `prescricao`, `valor_final`).
- `Usuario`: Credenciais de acesso com hash e perfis (`id_usuario`, `login`, `senha_hash`, `perfil`, `ativo`).

---

## 📁 5. Estrutura de Diretórios do Repositório
```text
plataforma-de-saude/
├── docs/                   # Documentação, Diagramas DER, UML e Tabela-Verdade (Passo 1 e 3)
├── prototype_cli/          # Módulo Python CLI — Menu interativo, cadastros e estatísticas (Passo 2)
├── database/               # Scripts SQL (DDL e Cargas Iniciais)
├── src/                    # Código-fonte da aplicação (Controllers, Models, Services, Views)
├── .gitignore              # Exclusão de arquivos temporários e caches
├── requirements.txt        # Dependências do ambiente Python
└── README.md               # Documentação principal do repositório
