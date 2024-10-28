# Notificação Automatizada de Relatórios Filtrados para Clientes

## Descrição
Este fluxo automatiza o envio diário de relatórios filtrados para clientes, aplicando RLS (Row-Level Security) para que cada cliente receba apenas as informações pertinentes a seu projeto.

## Estrutura do Fluxo
- **Trigger**: Agendamento diário (configurável)
- **Etapas principais**: Obter Projetos, Exportar Relatório do Power BI com RLS, Enviar E-mail

## Documentação Técnica
- **Parâmetros de Exportação**:
  ```json
  {
    "username": @{item()?['Solicitante_x003a__x0020_email/Value']},
    "datasets": ["88d00146-70e1-44f7-8510-95c9bf76f64e"],
    "roles": [@{item()?['Nome']}]
  }
  ```
- **Modelo de E-mail**:
  ```html
  <p>Olá,</p>
  <p>Segue abaixo a imagem do relatório referente ao projeto @{item()?['Nome']}:</p>
  <img alt="Relatório do Projeto" src="data:image/png;base64,@{base64(body('Exportar_Imagem_Relatório_Power_BI_com_RLS'))}">
  <p>Atenciosamente,<br>Assistente Virtual</p>
  ```

## Observações
A aplicação de RLS assegura que cada cliente visualize somente os dados relevantes, otimizando a segurança e a precisão da comunicação.
