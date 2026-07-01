# 🏥 Assistente Virtual Inteligente com IA e Orquestração de Processos (n8n)

Este projeto apresenta o desenvolvimento de um ecossistema completo e automatizado para triagem e suporte de atendimentos via WhatsApp, projetado especificamente para otimizar o fluxo de comunicação de clínicas de saúde.

## 🏗️ Arquitetura do Sistema e Tecnologias
O projeto foi totalmente construído utilizando uma arquitetura baseada em microsserviços conteinerizados via **Docker**, garantindo portabilidade e isolamento do ambiente.

* **n8n:** Orquestrador central responsável pelo desenho dos pipelines e fluxos lógicos de dados.
* **WAHA (WhatsApp HTTP API):** API de comunicação utilizada para ler e disparar mensagens de forma ativa.
* **DeepSeek (LLM):** Inteligência Artificial integrada como agente inteligente para interpretação de contexto e respostas humanizadas.
* **PostgreSQL:** Banco de dados relacional encarregado do armazenamento estruturado dos dados cadastrais dos pacientes e controle administrativo de estados.
* **Redis:** Banco de dados em memória de alta performance utilizado para a persistência do histórico de conversas (memória ativa do agente).

## 🛡️ Diferenciais Técnicos e Regras de Negócio
1.  **Sanitização de Dados:** Implementação de expressões JavaScript para higienizar chaves de metadados instáveis enviadas pela API da Meta, garantindo a integridade dos dados no banco.
2.  **Mecanismo "Porteiro da IA" (Gestão de Estado):** Criação de uma central invisível de comandos (`//pausar` e `//voltar`) disparada diretamente pelo chat. O sistema consulta dinamicamente o status no PostgreSQL através de condicionais para interromper ou liberar a IA, devolvendo o controle imediato ao atendimento humano quando necessário.
3.  **Triagem e Priorização:** O agente identifica gatilhos de interesse em consultas particulares e insere etiquetas de urgência (`[URGENTE - PARTICULAR]`) diretamente no histórico clínico do banco de dados para priorização na fila.
4.  **Sistema Carteiro:** Automação baseada em eventos (`Postgres Trigger`) configurada exclusivamente para escutar operações de `Insert` na tabela de pacientes, enviando notificações instantâneas de novos cadastros sem gerar spans de atualizações de rotina.

## 📂 Como Utilizar
Os arquivos de configuração estruturados dos fluxos lógicos encontram-se na pasta `/workflows`. Para utilizá-los:
1. Instale o ambiente utilizando as imagens Docker oficiais do n8n, WAHA, PostgreSQL e Redis.
2. Crie um novo fluxo em branco no seu painel do n8n.
3. Copie o conteúdo de um dos arquivos `.json` deste repositório e cole (`Ctrl+V`) diretamente na tela do editor do n8n.
4. Vincule as credenciais correspondentes de banco de dados e APIs.
