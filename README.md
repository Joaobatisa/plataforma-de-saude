# Plataforma de Saude
# 🏥 Plataforma de Gestão - Clínica Vida+

Sistema desenvolvido para informatizar e otimizar as rotinas operacionais, agendamentos, prontuários médicos e controle financeiro da **Clínica Vida+**, eliminando processos manuais e falhas de comunicação.

---

## 🎯 Objetivos do Projeto
- **Fim do Agendamento Manual:** Controle eletrônico de disponibilidade de horários médicos em tempo real.
- **Histórico Clínico Unificado:** Acesso rápido e seguro aos prontuários e evoluções dos pacientes pelos médicos.
- **Confiabilidade Administrativa:** Redução de erros em cobranças, emissão de comprovantes e relatórios gerenciais.
- **Gestão de Fila de Atendimento:** Organização lógica da ordem de chegada dos pacientes (estrutura FIFO).

---

## 🛠️ Arquitetura e Tecnologias
- **Linguagem Backend:** Python
- **Padrão de Projeto:** Arquitetura em Camadas / MVC (Models, Views/Controllers, Services)
- **Banco de Dados:** Relacional (SQL) com integridade referencial
- **Gestão Ágil & Versionamento:** Trello (Kanban) + Git & GitHub

---

## 📂 Estrutura de Diretórios
```text
plataforma-de-saude/
│
├── docs/                          # Documentação técnica, normas e diagramas UML
│   ├── normas_aceite.md           # Critérios de aceite alinhados às normas ISO/IEEE
│   ├── diagrama_casos_uso.png     # Modelagem UML de Casos de Uso (Atores e Interações)
│   └── diagrama_sequencia.png     # Fluxo temporal crítico de agendamento
│
├── src/                           # Código-fonte da aplicação (Backend Python)
├── .env.example                   # Modelo de variáveis de ambiente
├── .gitignore                     # Arquivos ignorados pelo controle de versão
├── requirements.txt               # Dependências do projeto Python
└── README.md                      # Documentação principal do repositório
