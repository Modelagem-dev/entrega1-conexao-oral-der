# Dicionário de Dados Conceitual — Conexão Oral Odontologia Especializada

> Este dicionário permanece no nível conceitual: identifica, descreve, classifica e associa regras de negócio aos atributos de cada entidade. Tipos de banco de dados, tamanhos e nulidade serão definidos nas etapas de modelo lógico e físico.
>
> Os exemplos de valores citados são fictícios, usados apenas para ilustrar o tipo de dado, e não representam pacientes ou profissionais reais da clínica.

---

## Entidade: PACIENTE

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_paciente | Identificador | Identifica unicamente o paciente. | Não pode se repetir; gerado pelo sistema. |
| nome_completo | Simples | Nome completo do paciente (ex.: "Maria Souza Lima"). | Obrigatório. |
| data_nascimento | Simples | Data de nascimento do paciente. | Obrigatório; usada para determinar se é menor de idade (RN02). |
| sexo | Simples | Sexo do paciente. | Obrigatório. |
| cpf | Simples | CPF do paciente (ex.: "123.456.789-00"). | Obrigatório para maiores de idade. |
| rg | Simples | RG do paciente. | Opcional. |
| profissao | Simples | Profissão do paciente. | Opcional. |
| estado_civil | Simples | Estado civil do paciente. | Opcional. |
| endereco | Composto | Endereço completo (rua, número, complemento, bairro, CEP). | Opcional. |
| telefone | Simples | Telefone/WhatsApp de contato. | Obrigatório; usado para confirmação de consultas. |
| email | Simples | E-mail de contato. | Opcional. |
| responsavel_legal | Composto | Nome e CPF do responsável legal. | Obrigatório apenas se o paciente for menor de idade (RN02). |
| historico_saude | Multivalorado | Respostas da avaliação de saúde (alergias, doenças, medicações, gravidez, etc.), conforme Ficha de Anamnese. | Obrigatório antes do primeiro atendimento (RN01). |
| data_cadastro | Simples | Data em que o paciente foi cadastrado no sistema. | Gerado automaticamente pelo sistema. |

---

## Entidade: DENTISTA

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_dentista | Identificador | Identifica unicamente o dentista. | Não pode se repetir; gerado pelo sistema. |
| nome | Simples | Nome completo do dentista (ex.: "Dr. João Pereira"). | Obrigatório. |
| cro | Simples | Número de registro profissional (CRO). | Obrigatório; deve ser único. |
| especialidade | Simples (domínio fechado) | Especialidade do dentista. | Obrigatório. Valores observados: ortodontia, endodontia, implantodontia, HOF, prótese, periodontia, dentística. |
| telefone | Simples | Telefone de contato profissional. | Opcional. |

---

## Entidade: CONSULTA

> Entidade unificada — reúne os dados de agendamento e os dados clínicos do atendimento, já que na prática da clínica esses dois momentos são tratados como um só evento (ver Seção 17.1 do README).

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_consulta | Identificador | Identifica unicamente a consulta. | Não pode se repetir; gerado pelo sistema. |
| id_paciente | Simples (referência) | Paciente vinculado à consulta. | Obrigatório (RN04). |
| id_dentista | Simples (referência) | Dentista responsável pela consulta. | Obrigatório (RN04). |
| data | Simples | Data marcada para a consulta. | Obrigatório; deve respeitar o horário de funcionamento (RP01). |
| horario | Simples | Horário marcado. | Obrigatório; não pode haver conflito de horário para o mesmo dentista. |
| status | Simples (domínio fechado) | Situação da consulta. | Obrigatório (RN03). Valores: agendada, confirmada, realizada, não compareceu, cancelada. |
| procedimento | Simples | Procedimento realizado (ex.: "restauração", "limpeza", "extração"). | Preenchido no momento do atendimento. |
| observacoes | Simples | Observações clínicas do atendimento. | Opcional. |
| tipo_pagamento | Simples (domínio fechado) | Indica se a consulta é particular ou por convênio. | Obrigatório; define se será gerada Guia ou Pagamento (RN09). |

---

## Entidade: GUIA

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_guia | Identificador | Identifica unicamente a guia. | Não pode se repetir; gerado pelo sistema. |
| id_consulta | Simples (referência) | Consulta vinculada à guia. | Obrigatório. |
| id_convenio | Simples (referência) | Convênio vinculado à guia. | Obrigatório. |
| data_emissao | Simples | Data de emissão da guia. | Obrigatório. |
| assinada | Simples (booleano) | Indica se o paciente já assinou a guia. | Obrigatório; guia só pode ser enviada ao convênio se assinada (RN05). |
| data_assinatura | Simples | Data em que a assinatura foi registrada. | Preenchido apenas após assinatura. |

---

## Entidade: CONVENIO

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_convenio | Identificador | Identifica unicamente o convênio. | Não pode se repetir; gerado pelo sistema. |
| nome | Simples | Nome do convênio (ex.: "Amil Dental", "OdontoPrev"). | Obrigatório. |
| contato | Simples | Telefone/e-mail de contato do convênio. | Opcional. |

---

## Entidade: PAGAMENTO

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_pagamento | Identificador | Identifica unicamente o pagamento. | Não pode se repetir; gerado pelo sistema. |
| id_consulta | Simples (referência) | Consulta vinculada ao pagamento. | Obrigatório. |
| valor | Simples | Valor pago pelo paciente. | Obrigatório. |
| forma_pagamento | Simples (domínio fechado) | Forma de pagamento (ex.: "cartão", "pix", "dinheiro"). | Obrigatório. |
| data_pagamento | Simples | Data em que o pagamento foi realizado. | Obrigatório; registrado no encerramento da consulta (RN07). |

---

## Entidade: PECA_PROTETICA

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_peca | Identificador | Identifica unicamente a peça protética. | Não pode se repetir; gerado pelo sistema. |
| id_consulta | Simples (referência) | Consulta que originou a solicitação da peça. | Obrigatório. |
| id_laboratorio | Simples (referência) | Laboratório responsável pela produção. | Obrigatório (RN08). |
| tipo_peca | Simples | Tipo de peça solicitada (ex.: "prótese total", "faceta"). | Obrigatório. |
| prazo_estimado | Simples | Data estimada de entrega pelo laboratório. | Obrigatório. |
| data_entrega_real | Simples | Data em que a peça foi efetivamente entregue. | Preenchido apenas após entrega. |
| status | Simples (domínio fechado) | Situação da peça. | Obrigatório (RN06). Valores: solicitada, em produção, entregue, atrasada. |

---

## Entidade: LABORATORIO_PROTETICO

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_laboratorio | Identificador | Identifica unicamente o laboratório. | Não pode se repetir; gerado pelo sistema. |
| nome | Simples | Nome do laboratório protético. | Obrigatório. |
| contato | Simples | Telefone/e-mail de contato do laboratório. | Opcional. |

---

## Entidade: RECEITUARIO

| Atributo | Classificação | Descrição | Regra de negócio associada |
|---|---|---|---|
| id_receituario | Identificador | Identifica unicamente o receituário. | Não pode se repetir; gerado pelo sistema. |
| id_consulta | Simples (referência) | Consulta que originou a prescrição. | Obrigatório; só pode existir vinculado a uma consulta já realizada (RN10). |
| data_emissao | Simples | Data de emissão do receituário. | Obrigatório. |
| tipo_receita | Simples (domínio fechado) | Tipo de receita (ex.: "simples", "controlada"). | Obrigatório. |
| medicamento | Simples | Nome do medicamento prescrito. | Obrigatório. |
| posologia | Simples | Instruções de dosagem e frequência. | Obrigatório. |
| orientacoes | Simples | Orientações adicionais ao paciente. | Opcional. |
