# Plataforma de Saúde
# Plataforma de Gestão - Clínica Vida+

Sistema desenvolvido para informatizar e otimizar as rotinas operacionais, agendamentos, prontuários médicos e controle financeiro da **Clínica Vida+**, eliminando processos manuais e falhas de comunicação.

---

## 🎯 Objetivos do Projeto
- **Fim do Agendamento Manual:** Controle eletrônico de disponibilidade de horários médicos em tempo real.
- **Histórico Clínico Unificado:** Acesso rápido e seguro aos prontuários e evoluções dos pacientes pelos médicos.
- **Confiabilidade Administrativa:** Redução de erros em cobranças, emissão de comprovantes e relatórios gerenciais.
- **Gestão de Fila de Atendimento:** Organização lógica da ordem de chegada dos pacientes (estrutura FIFO).
- **Continuidade e Segurança dos Dados (Nuvem):** Sincronização e backup automático em nuvem para evitar perda de informações em caso de queda de energia na clínica ou desligamento/falta de bateria no celular do paciente durante o recebimento de consultas e exames, permitindo restauração rápida do sistema.

---

## 🛠️ Arquitetura e Tecnologias
- **Linguagem Backend & API:** Python (com integração via API REST para comunicação entre Desktop e Mobile)
- **Plataforma de Execução:** Aplicação Desktop (posto de atendimento da clínica) + Acesso Mobile/Web para pacientes
- **Tecnologia Frontend / Interface:** HTML5, CSS3, JavaScript e Templates
- **Padrão de Projeto:** Arquitetura em Camadas / MVC (Modelos, Views/Controllers, Serviços)
- **Banco de Dados & Nuvem:** Relacional (SQL) com XAMPP/MySQL em ambiente de desenvolvimento e sincronização/backup em Nuvem para redundância e recuperação de desastres
- **Ambiente de Desenvolvimento:** Visual Studio Code (IDE)
- **Gestão Ágil e Versionamento:** Trello (Kanban) + Git & GitHub

---

## 📂 Estrutura de Diretórios
```text
plataforma-de-saude/
│
├── src/                       # Código-fonte da aplicação (Backend Python, Desktop e API)
├── templates/                 # Páginas HTML / Interface de Visualização (Frontend)
├── .env.example               # Modelo de variáveis de ambiente (chaves de API e Nuvem)
├── .gitignore                 # Arquivos ignorados pelo controle de versão
├── requirements.txt           # Dependências do projeto Python
└── README.md                  # Documentação principal do repositório
