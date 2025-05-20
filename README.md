# Sistema de Gerenciamento de Ordem de Serviço em Oficina Mecânica

Este repositório apresenta o modelo conceitual de um banco de dados projetado para um sistema de controle e gerenciamento de Ordens de Serviço (OS) em uma oficina mecânica. O principal objetivo deste projeto é otimizar o fluxo de trabalho da oficina, desde o momento em que um veículo é recebido para manutenção até a conclusão e entrega dos serviços.

## Descrição do Desafio

O desafio proposto consistiu em desenvolver um esquema conceitual de banco de dados. Para isso, foi utilizada uma narrativa detalhada que descrevia os processos e requisitos de uma oficina mecânica. A partir dessa narrativa, foram identificadas e modeladas todas as entidades, seus respectivos atributos e os relacionamentos existentes entre elas. Nos casos em que a narrativa não fornecia detalhes suficientes, decisões foram tomadas com base no entendimento do contexto de uma oficina, e essas decisões são explicadas em uma seção específica deste README.

## Objetivo

O objetivo central deste projeto é a criação de um esquema conceitual de banco de dados robusto e funcional, que sirva como base para a implementação de um sistema de gerenciamento de ordens de serviço para uma oficina mecânica, alinhado com a narrativa e os requisitos levantados.

## Narrativa Base (Requisitos do Projeto)

A seguir, estão os principais pontos da narrativa que guiaram a construção do modelo:

* O sistema foca no controle e gerenciamento da execução de ordens de serviço em uma oficina mecânica.
* Clientes levam seus veículos à oficina para consertos ou revisões periódicas.
* Cada veículo que entra na oficina é designado a uma equipe de mecânicos. Esta equipe é responsável por identificar os serviços necessários e preencher a Ordem de Serviço (OS), incluindo uma data prevista para a entrega.
* O valor de cada serviço na OS é calculado com base em uma tabela de referência de mão-de-obra.
* O valor das peças utilizadas também é considerado na composição do valor total da OS.
* É fundamental que o cliente autorize a execução dos serviços antes que eles sejam iniciados.
* A mesma equipe de mecânicos que avalia os serviços é a responsável por executá-los.
* Os mecânicos da oficina possuem atributos como código, nome, endereço e especialidade.
* Cada Ordem de Serviço (OS) deve conter um número de identificação, data de emissão, valor total, status atual e uma data para a conclusão dos trabalhos.
* Uma única OS pode abranger múltiplos serviços, e um mesmo tipo de serviço pode ser aplicado em diversas OSs.
* Da mesma forma, uma OS pode requerer vários tipos de peças, e um mesmo tipo de peça pode ser utilizado em múltiplas OSs.

## Entidades e Atributos Detalhados

### Cliente
* `ID_Cliente`: Chave Primária (PK). Identificador único para cada cliente.
* `Nome`: Nome completo do cliente.
* `Endereco`: Endereço de residência ou comercial do cliente.
* `Telefone`: Número de contato do cliente.
* `Email`: Endereço de e-mail do cliente.

### Veiculo
* `ID_Veiculo`: Chave Primária (PK). Identificador único para cada veículo.
* `Placa`: Placa de identificação do veículo.
* `Marca`: Fabricante do veículo (ex: Ford, Chevrolet).
* `Modelo`: Modelo específico do veículo (ex: Focus, Onix).
* `Ano`: Ano de fabricação do veículo.
* `ID_Cliente`: Chave Estrangeira (FK). Referencia o `ID_Cliente` da entidade `Cliente`, indicando o proprietário do veículo.

### Mecanico
* `ID_Mecanico`: Chave Primária (PK). Identificador único para cada mecânico.
* `Codigo`: Código interno de identificação do mecânico.
* `Nome`: Nome completo do mecânico.
* `Endereco`: Endereço de residência do mecânico.
* `Especialidade`: Área de especialização do mecânico (ex: Elétrica, Motor, Suspensão).

### Equipe_Mecanicos
* `ID_Equipe`: Chave Primária (PK). Identificador único para cada equipe de mecânicos.
* `Nome_Equipe`: Nome ou designação da equipe (ex: Equipe Alfa, Equipe Noturna).

### Mecanico_Equipe (Tabela Associativa)
Esta tabela resolve o relacionamento de N:M entre `Mecanico` e `Equipe_Mecanicos`.
* `ID_Mecanico`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_Mecanico`.
* `ID_Equipe`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_Equipe`.

### Ordem_Servico (OS)
* `ID_OS`: Chave Primária (PK). Identificador único para cada Ordem de Serviço.
* `Numero`: Número sequencial ou de protocolo da OS.
* `Data_Emissao`: Data em que a OS foi criada.
* `Valor`: Valor total calculado da Ordem de Serviço (serviços + peças).
* `Status`: Situação atual da OS (ex: Aberta, Em Andamento, Aguardando Peças, Concluída, Cancelada).
* `Data_Conclusao`: Data real em que os trabalhos na OS foram finalizados.
* `Data_Entrega_Prevista`: Data estimada para a entrega do veículo ao cliente.
* `ID_Veiculo`: Chave Estrangeira (FK). Referencia o `ID_Veiculo` da entidade `Veiculo`.
* `ID_Equipe`: Chave Estrangeira (FK). Referencia o `ID_Equipe` da entidade `Equipe_Mecanicos`, indicando a equipe responsável pela OS.
* `Autorizacao_Cliente`: Campo booleano (verdadeiro/falso) indicando se o cliente autorizou formalmente a execução dos serviços.

### Servico
* `ID_Servico`: Chave Primária (PK). Identificador único para cada tipo de serviço.
* `Descricao`: Nome ou descrição detalhada do serviço (ex: Troca de Óleo, Alinhamento e Balanceamento).
* `Valor_Tabela_Mao_Obra`: Valor de referência para a mão de obra deste serviço.

### Item_OS_Servico (Tabela Associativa)
Esta tabela resolve o relacionamento de N:M entre `OS` e `Servico`.
* `ID_OS`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_OS`.
* `ID_Servico`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_Servico`.
* `Valor_Calculado_Servico`: O valor específico calculado para este serviço dentro da Ordem de Serviço, considerando possíveis descontos ou ajustes.

### Peca
* `ID_Peca`: Chave Primária (PK). Identificador único para cada peça.
* `Nome_Peca`: Nome comercial da peça (ex: Filtro de Ar, Pastilha de Freio).
* `Descricao`: Descrição detalhada da peça.
* `Valor_Unitario`: Preço unitário da peça.

### Item_OS_Peca (Tabela Associativa)
Esta tabela resolve o relacionamento de N:M entre `OS` e `Peca`.
* `ID_OS`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_OS`.
* `ID_Peca`: Chave Estrangeira (FK) e parte da Chave Primária composta. Referencia o `ID_Peca`.
* `Quantidade`: Quantidade daquela peça específica utilizada na Ordem de Serviço.
* `Valor_Total_Peca`: O valor total da peça para aquela OS (Quantidade * Valor Unitário).

## Relacionamentos Principais

* **Cliente (1) --- N (Veiculo)**: Um cliente pode possuir um ou vários veículos.
* **Veiculo (1) --- N (Ordem_Servico)**: Um veículo pode gerar uma ou várias ordens de serviço ao longo do tempo.
* **Mecanico (N) --- M (Equipe_Mecanicos)** (via `Mecanico_Equipe`): Um mecânico pode fazer parte de múltiplas equipes, e uma equipe pode ser composta por vários mecânicos.
* **Equipe_Mecanicos (1) --- N (Ordem_Servico)**: Cada ordem de serviço é atribuída a uma única equipe de mecânicos.
* **Ordem_Servico (N) --- M (Servico)** (via `Item_OS_Servico`): Uma ordem de serviço pode incluir múltiplos serviços, e um mesmo tipo de serviço pode aparecer em diferentes ordens de serviço.
* **Ordem_Servico (N) --- M (Peca)** (via `Item_OS_Peca`): Uma ordem de serviço pode utilizar vários tipos de peças, e uma mesma peça pode ser empregada em diferentes ordens de serviço.

## Considerações e Decisões Tomadas 

Durante o processo de modelagem, algumas decisões foram tomadas para complementar as informações da narrativa e garantir a integridade e completude do banco de dados:

* **Atributos de Contato do Cliente**: Embora não explicitamente mencionados, `Telefone` e `Email` foram adicionados à entidade `Cliente`. Estes são atributos padrão e essenciais para a comunicação e relacionamento com os clientes em qualquer sistema de gerenciamento.
* **Modelagem de Equipes e Mecânicos**: A narrativa indicava que "a mesma equipe avalia e executa os serviços" e que "cada veículo é designado a uma equipe de mecânicos". Para representar que uma equipe é composta por vários mecânicos e que um mecânico pode pertencer a várias equipes (relação N:M), foi criada a entidade `Equipe_Mecanicos` e uma tabela associativa `Mecanico_Equipe`.
* **Representação da "Tabela de Referência de Mão-de-Obra"**: A menção de que o valor do serviço é calculado "consultando-se uma tabela de referência de mão-de-obra" foi interpretada como um atributo (`Valor_Tabela_Mao_Obra`) dentro da própria entidade `Servico` para simplificar o modelo conceitual. Em uma implementação real, isso poderia ser uma tabela separada de preços de serviços.
* **Detalhamento de Valores em Itens da OS**: Para permitir um cálculo preciso do valor total da OS, atributos como `Valor_Calculado_Servico` na tabela `Item_OS_Servico` e `Valor_Total_Peca` na tabela `Item_OS_Peca` foram incluídos. Isso permite que se registrem os valores específicos aplicados àquela OS, que podem diferir do valor de tabela devido a promoções ou negociações.
* **Autorização do Cliente**: A frase "O cliente autoriza a execução dos serviços" foi modelada como um campo booleano (`Autorizacao_Cliente`) na entidade `Ordem_Servico` para registrar essa etapa crucial do processo.
* **Distinção entre `Data_Conclusao` e `Data_Entrega_Prevista`**: A narrativa menciona "data para conclusão dos trabalhos" e que a equipe "preenche uma OS com data de entrega". Embora possam ser a mesma, é comum que a data de entrega seja uma previsão informada ao cliente, enquanto a data de conclusão é quando o serviço realmente terminou. Para capturar ambas as nuances, `Data_Entrega_Prevista` foi adicionada separadamente.

