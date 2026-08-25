# Documento de Especificação de Requisitos

**Projeto:** Sistema de Gestão de Agendamentos Comerciais

**Autor:** Ryan De Oliveira Carvalho

## 1 Introdução
O Sistema de Gestão de Agendamentos Comerciais tem como finalidade digitalizar e organizar o processo de reserva de horários para prestação de serviços ou uso de recursos em diferentes tipos de estabelecimentos, como clínicas, hospitais, barbearias e restaurantes.


### 1.1 Objetivo
Apresentar de forma detalhada os requisitos funcionais e não funcionais do Sistema de Gestão de Agendamentos Comerciais, servindo como a "planta" do projeto que guiará a estruturação das classes e o desenvolvimento do software em POO.

### 1.2 Escopo do produto
O escopo deste documento abrange as funcionalidades e características do Sistema de Gestão de Agendamentos Comerciais, desde a configuração do expediente e cadastro de serviços pelo gerente, passando pela busca de disponibilidade e realização do agendamento pelo cliente, até o gerenciamento da agenda diária e alteração de status das reservas pelo prestador de serviço.

---

## 2 Requisitos Funcionais

| Código | Nome | Descrição |
| :--- | :--- | :--- |
| **RF01** | Cadastro de Serviços e Recursos | O Gerente deve ser capaz de cadastrar os serviços ou recursos oferecidos, informando nome, duração estimada e valor (se aplicável). |
| **RF02** | Configuração de Expediente | O Gerente deve poder definir a grade padrão de horários de funcionamento do estabelecimento. |
| **RF03** | Visualização de Disponibilidade | O sistema deve calcular e exibir para o Cliente apenas os horários livres, considerando a duração do serviço escolhido e os agendamentos já confirmados. |
| **RF04** | Agendamento | O Cliente deve poder realizar a reserva de um serviço/recurso em um horário disponível. |
| **RF05** | Gestão de Status | O Prestador ou Gerente deve poder alterar o status do agendamento (Ex: Pendente, Confirmado, Concluído, Cancelado, No-show). |
| **RF06** | Cancelamento | O Cliente deve poder cancelar seu agendamento previamente realizado. |

---

## 3 Requisitos Não Funcionais

### 3.1 Desempenho
| Código | Requisito / Aplicação | Descrição |
| :--- | :--- | :--- |
| **RNF01** | Aplicação | O cálculo e exibição de horários disponíveis devem ocorrer de forma imediata para evitar que dois clientes disputem a mesma vaga simultaneamente. |

### 3.2 Segurança
| Código | Requisito / Aplicação | Descrição |
| :--- | :--- | :--- |
| **RNF02** | Isolamento de Dados | O sistema deve garantir que os dados de agendamentos, prestadores e clientes de um estabelecimento não possam, em hipótese alguma, ser acessados por outro (arquitetura multi-tenant). |

### 3.3 Usabilidade
| Código | Requisito / Aplicação | Descrição |
| :--- | :--- | :--- |
| **RNF03** | Interface Intuitiva | O fluxo de agendamento para o Cliente deve ser simples, claro e ser concluído em poucos passos para garantir a conversão da reserva. |

---

## 4 Casos de Uso

### Caso de Uso: Agendar Serviço/Recurso

*   **ATOR PRINCIPAL:** Cliente
*   **PRÉ-CONDIÇÃO:** O estabelecimento deve ter horários configurados e o Cliente deve estar autenticado.
*   **FLUXO PRINCIPAL:**
    1. O Cliente acessa a página do estabelecimento.
    2. O Cliente seleciona o serviço ou recurso desejado.
    3. O Cliente (opcionalmente) seleciona o prestador de serviço de sua preferência.
    4. O sistema processa a agenda e exibe os horários disponíveis (calculados com base na duração do serviço).
    5. O Cliente escolhe um dia e horário e clica em confirmar.
    6. O sistema registra a reserva no banco de dados e notifica o estabelecimento.
*   **FLUXO ALTERNATIVO:**
    *   Caso o horário seja reservado por outra pessoa no exato momento da confirmação (concorrência), o sistema emite um alerta e solicita nova escolha de horário.
*   **PÓS-CONDIÇÃO:** Agendamento criado com status "Confirmado" e alocado na grade do prestador/recurso correspondente.
*   **RELAÇÃO:** `<<include>>` Selecionar Serviço `<<include>>` Selecionar Horário.

---

## 5 Regras de Negócio

*   **RN01: Prevenção de Conflitos (Overbooking):** O sistema é estritamente proibido de permitir dois agendamentos confirmados para o mesmo recurso físico (ex: mesma mesa, mesma sala) ou mesmo prestador humano em horários que se sobreponham.
*   **RN02: Cálculo de Encaixe Lógico:** Um bloco de horário só é considerado "disponível" se houver tempo hábil entre o início pretendido e o próximo compromisso do prestador (ou o horário de fechamento do negócio), sendo este tempo livre maior ou igual à duração do serviço selecionado.
*   **RN03: Janela Limite de Cancelamento:** O Cliente só pode efetuar o cancelamento de forma autônoma pelo sistema com uma antecedência mínima configurada pelo Gerente (ex: até 4 horas antes do evento). Após esse prazo, o cancelamento é bloqueado no aplicativo.