# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: João Miguel da Silva Cavalcante 
RA: 25001039 
Data: 25/03/2026 

---

# 1. Definição do MVP

O MVP (Minimum Viable Product) foca no core operacional da farmácia: o ciclo de venda no balcão, a integridade do estoque físico e o registro financeiro básico (contas a pagar/receber).
Dentro do MVP: Cadastro de produtos/clientes, realização de vendas (à vista e a prazo), controle de estoque automático, registro de compras e validação de receitas.
Fora do MVP: Relatórios avançados de BI (Business Intelligence), integração com convênios externos de grande porte e sistema de fidelidade por pontos.
Justificativa: Estas escolhas garantem que a farmácia pare de perder dinheiro com furos de estoque e falhas de lançamentos financeiros imediatos.

---

# 2. Regras de Negócio:

**RN01 — Retenção de Receita: Medicamentos controlados só podem ter a venda finalizada mediante validação do farmacêutico e registro dos dados da receita.**
**RN02 — Estoque Mínimo: O sistema deve impedir a venda de produtos com quantidade insuficiente em estoque.**
**RN03 — Venda a Prazo: Somente clientes com cadastro aprovado e sem pendências financeiras podem realizar compras na modalidade "A Prazo".**
**RN04 — Atualização de Saldo: Qualquer movimentação (venda, perda ou devolução) deve atualizar o saldo do estoque da unidade em tempo real.**
**RN05 — Registro de Contas: Toda compra de fornecedor deve gerar automaticamente um lançamento em "Contas a Pagar".**

---

# 3. Requisitos Funcionais:

**RF01 — Cadastro de Produtos: O sistema deve permitir o registro de produtos com descrição, preço, fabricante e unidade de medida.**
**RF02 — Busca Ágil: O atendente deve conseguir pesquisar produtos por nome ou código de barras.**
**RF03 — Registro de Clientes: O sistema deve permitir o cadastro rápido de clientes no momento da venda.**
**RF04 — Baixa de Estoque: O sistema deve subtrair os itens vendidos do estoque da unidade após a confirmação da venda.**
**RF05 — Alerta de Estoque Crítico: O sistema deve emitir avisos quando um produto atingir o nível mínimo definido pelo gerente.**
**RF06 — Gestão de Contas a Receber: O sistema deve listar vendas a prazo com status "Aberta", "Recebida" ou "Atrasada".**
**RF07 — Gestão de Contas a Pagar: O sistema deve registrar pagamentos a fornecedores com data de vencimento.**
**RF08 — Emissão de Comprovante: O sistema deve gerar um comprovante detalhado ao final de cada transação.**

---

# 🛡 4. Requisitos Não Funcionais:

**RNF01 — Segurança: O acesso ao sistema deve ser restrito por login e senha, com permissões baseadas no perfil (Atendente, Farmacêutico, etc.).**
**RNF02 — Performance: A consulta de estoque deve retornar resultados em menos de 2 segundos.**
**RNF03 — Disponibilidade: O sistema deve operar em nuvem com disponibilidade de 99,8% para acesso das filiais.**
**RNF04 — Auditabilidade: Toda alteração manual no estoque deve registrar o usuário responsável e o motivo.**

---

# 5. Casos de Uso 

![Diagrama de Casos de Uso Geral](img/diagrama_geral.png)

# 6. Documentação dos Casos de Uso

## UC01 — Realizar Venda

Ator(es): Atendente.
Descrição: Registrar a saída de produtos para um cliente mediante pagamento.
Pré-condições: Usuário autenticado; Produto cadastrado e com saldo em estoque.
Pós-condições: Estoque atualizado; Venda registrada; Comprovante emitido.
Fluxo Principal:

Atendente inicia a venda.

Atendente pesquisa o produto (Include: UC02).

Atendente verifica se há saldo disponível (Include: UC03).

Atendente informa a quantidade e adiciona ao carrinho.

Sistema calcula o valor total.

Atendente finaliza o pagamento e o sistema emite o comprovante.
Fluxos Alternativos / Exceções:

FA01 — Medicamento Controlado: O sistema exige a validação do farmacêutico (Extend: UC04).
FA02 — Venda a Prazo: O cliente opta por pagar depois, gerando título financeiro (Extend: UC05).
Relacionamentos:
Include: UC02, UC03.
Extend: UC04, UC05.

![Diagrama de Atividades UC01](img/uc01_atividades.png)

## UC02 — Pesquisar Produto
Ator(es): Atendente, Gerente, Farmacêutico.
Descrição: Localizar um item no sistema por nome, código de barras ou fabricante.
Pré-condições: Acesso ao módulo de consulta.
Pós-condições: Informações do produto exibidas na tela.
Fluxo Principal:

Usuário insere o termo de busca.

Sistema filtra a base de dados.

Sistema exibe descrição, preço e fabricante.
Fluxos Alternativos / Exceções:

FA01 — Produto Inexistente: Sistema informa que o código não foi encontrado.
Relacionamentos:

Include: Nenhum.

Extend: Nenhum.

![Diagrama de Atividades UC02](img/uc02_atividades.png)

## UC03 — Verificar Estoque
Ator(es): Sistema.
Descrição: Validar se a quantidade solicitada está disponível na unidade atual.
Pré-condições: Produto identificado.
Pós-condições: Quantidade validada para venda.
Fluxo Principal:

Sistema consulta o saldo atual do item.

Sistema compara saldo com a quantidade desejada.

Sistema autoriza a reserva temporária do item.
Fluxos Alternativos / Exceções:

FA01 — Estoque Insuficiente: Sistema bloqueia a inserção e sugere consulta em outra unidade.
Relacionamentos:

Include: Nenhum.

Extend: Nenhum.

![Diagrama de Atividades UC03](img/uc03_atividades.png)

## UC04 — Validar Receita Médica
Ator(es): Farmacêutico.
Descrição: Analisar e autorizar a venda de medicamentos de uso controlado.
Pré-condições: Venda em andamento; Item marcado como controlado.
Pós-condições: Venda liberada ou bloqueada.
Fluxo Principal:

Farmacêutico analisa a receita física.

Farmacêutico insere os dados da receita (CRM, data, paciente) no sistema.

Farmacêutico autoriza a liberação do item no carrinho de vendas.
Fluxos Alternativos / Exceções:

FA01 — Receita Vencida/Inválida: Farmacêutico nega a autorização e o item é removido da venda.
Relacionamentos:

Extend: Estende UC01.

![Diagrama de Atividades UC04](img/uc04_atividades.png)

## UC05 — Registrar Contas a Receber
Ator(es): Sistema / Financeiro.
Descrição: Criar um título financeiro para vendas realizadas a prazo ou convênios.
Pré-condições: Venda finalizada na modalidade "A Prazo".
Pós-condições: Lançamento financeiro criado com status "Aberta".
Fluxo Principal:

Sistema identifica o cliente ou empresa conveniada.

Sistema gera as parcelas com datas de vencimento.

Sistema registra o valor total no módulo financeiro.
Fluxos Alternativos / Exceções:

FA01 — Cliente com Débito: O sistema impede o registro se houver faturas atrasadas (Extend: UC06).
Relacionamentos:

Extend: Estende UC01 e UC06.

![Diagrama de Atividades UC05](img/uc05_atividades.png)

## UC06 — Bloquear Cliente Inadimplente
Ator(es): Sistema.
Descrição: Impedir novas vendas a prazo para clientes com faturas em atraso.
Pré-condições: Tentativa de venda a prazo para cliente cadastrado.
Pós-condições: Venda bloqueada ou autorizada mediante pagamento da dívida.
Fluxo Principal:

Sistema verifica o histórico financeiro do cliente.

Sistema detecta parcelas com status "Atrasada".

Sistema emite alerta e bloqueia a finalização da venda.
Relacionamentos:

Extend: Estende UC05.

![Diagrama de Atividades UC06](img/uc06_atividades.png)

## UC07 — Registrar Compra de Fornecedor
Ator(es): Gerente.
Descrição: Lançar a entrada de mercadorias adquiridas para reposição.
Pré-condições: Nota fiscal em mãos; Fornecedor cadastrado.
Pós-condições: Estoque atualizado; Conta a pagar gerada.
Fluxo Principal:

Gerente informa os produtos e quantidades recebidas.

Gerente insere o valor total e o fornecedor.

Sistema atualiza o saldo do estoque (Include: UC08).

Sistema gera o lançamento no contas a pagar.
Relacionamentos:

Include: UC08.

![Diagrama de Atividades UC07](img/uc07_atividades.png)

## UC08 — Atualizar Saldo de Estoque
Ator(es): Sistema.
Descrição: Incrementar ou decrementar o saldo físico de produtos.
Pré-condições: Venda finalizada, Devolução ou Compra registrada.
Pós-condições: Saldo atualizado na base de dados.
Fluxo Principal:

Sistema identifica a operação (Entrada/Saída).

Sistema soma ou subtrai a quantidade do saldo anterior.

Sistema registra o log da movimentação (quem, quando e quanto).
Relacionamentos:

Include: Faz parte de UC01 e UC07.

![Diagrama de Atividades UC08](img/uc08_atividades.png)

## UC09 — Manter Cadastro de Produto
Ator(es): Gerente.
Descrição: Incluir, alterar ou excluir informações de produtos no catálogo.
Pré-condições: Usuário com perfil de Gerente.
Pós-condições: Dados do produto salvos permanentemente.
Fluxo Principal:

Gerente acessa o módulo de produtos.

Gerente preenche descrição, preço de venda e nível mínimo de estoque.

Sistema salva as alterações.
Relacionamentos:

Include: Nenhum.

![Diagrama de Atividades UC09](img/uc09_atividades.png)

## UC10 — Gerar Relatório de Desempenho
Ator(es): Gerente, Administrativo.
Descrição: Extrair dados de vendas e estoque para tomada de decisão.
Pré-condições: Acesso ao módulo de relatórios.
Pós-condições: Relatório gerado em tela ou PDF.
Fluxo Principal:

Usuário seleciona o tipo de relatório (ex: Mais Vendidos).

Usuário define o período de data.

Sistema processa os dados e exibe os indicadores.
Relacionamentos:

Include: Nenhum.

![Diagrama de Atividades UC10](img/uc10_atividades.png)


