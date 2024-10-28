#### 1. Descrição Geral

- **Nome**: Notificação Automatizada de Relatórios Filtrados para Clientes
- **Objetivo**: Automatizar a notificação diária de relatórios personalizados para clientes, aplicando RLS (Row-Level Security) para que cada cliente receba apenas as informações pertinentes ao seu projeto.
- **Visão Geral**: O fluxo consulta uma lista de projetos no SharePoint, aplica filtros específicos no Power BI usando RLS, exporta uma imagem do relatório customizado e envia automaticamente por e-mail para cada cliente. Esse processo garante segurança e precisão, com cada cliente recebendo somente o que lhe é relevante.

#### 2. Funcionalidades e Benefícios

- **Automação e Segurança de Dados**: A aplicação do RLS permite que cada cliente veja apenas seus dados, protegendo informações sensíveis e eliminando a necessidade de manutenções manuais para separação de dados.
- **Personalização**: Cada e-mail enviado é customizado com o nome do projeto e informações específicas ao cliente.
- **Execução Diária**: Agendamento automático diário, garantindo que os clientes recebam atualizações sempre com informações recentes.

#### 3. Estrutura do Fluxo

**Diagrama Simplificado**:

```
Start > Disparar um fluxo agendado diariamente > Obter lista de Projetos > For Each [ Exportar Imagem para Relatório do Power BI com RLS > Enviar E-mail (V2) ] > End
```

#### 4. Detalhamento das Etapas

##### **Etapa 1**: Disparar o Fluxo Agendado
- **Descrição**: Inicializa o fluxo em horário determinado diariamente.
- **Parâmetros**: Agendamento diário (horário configurável).

##### **Etapa 2**: Obter Lista de Projetos
- **Descrição**: Extrai informações dos projetos, incluindo Nome, Descrição, Solicitante e E-mail do Solicitante, a partir de uma lista SharePoint.
- **Fonte**: Lista de Projetos no SharePoint.
  
##### **Etapa 3**: For Each - Para cada projeto
  - **Descrição**: Executa as ações abaixo para cada item da lista de projetos.

###### **Etapa 3.1**: Exportar Imagem para Relatório do Power BI com RLS
  - **Descrição**: Exporta uma imagem do dashboard do Power BI aplicando RLS, assegurando que cada cliente receba apenas os dados pertinentes.
  - **Parâmetros**:
    ```json
    {
      "username": @{item()?['Solicitante_x003a__x0020_email/Value']},
      "datasets": ["88d00146-70e1-44f7-8510-95c9bf76f64e"],
      "roles": [@{item()?['Nome']}]
    }
    ```
    
###### **Etapa 3.2**: Enviar E-mail (V2)
  - **Descrição**: Envia um e-mail para o solicitante com o relatório filtrado em anexo.
  - **Corpo do E-mail**:
    ```html
    <p>Olá,</p>
    <p>Segue abaixo a imagem do relatório referente ao projeto @{item()?['Nome']}:</p>
    <img alt="Relatório do Projeto" src="data:image/png;base64,@{base64(body('Exportar_Imagem_Relatório_Power_BI_com_RLS'))}">
    <p>Atenciosamente,<br>Assistente Virtual</p>
    ```

---
