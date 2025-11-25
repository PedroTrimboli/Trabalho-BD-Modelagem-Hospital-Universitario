# 📘 Modelo Conceitual (MER) — Sistema de Gestão do Hospital Universitário

Este é o Modelo Entidade-Relacionamento (MER) simples, adequado para explicação oral e para projetos de primeiro semestre.

---

## 🧍 Entidade: PACIENTE

| Tipo de Atributo | Nome |
|------------------|------|
| **PK** | id_paciente |
| Simples | cpf |
| Simples | nome |
| Simples | data_nascimento |
| **Derivado** | idade (calculado a partir da data_nascimento) |
| **Multivalorado** | telefones |
| **Composto** | endereco (rua, numero, bairro, cidade, cep) |
| Simples | plano_saude |

---

## 🩺 Entidade: PROFISSIONAL

| Tipo de Atributo | Nome |
|------------------|------|
| **PK** | id_profissional |
| Simples | nome |
| Simples | tipo_profissional (médico, enfermeiro, técnico...) |
| Simples | registro_conselho (CRM, COREN, etc.) |
| Simples | especialidade |
| Simples | email |

---

## 🏥 Entidade: SETOR

| Tipo de Atributo | Nome |
|------------------|------|
| **PK** | id_setor |
| Simples | nome_setor |
| Simples | andar |

---

## 🛏️ Entidade: LEITO

| Tipo de Atributo | Nome |
|------------------|------|
| **PK** | id_leito |
| Simples | codigo_leito |
| Simples | status_leito (Livre, Ocupado, Em limpeza) |
| **FK** | id_setor (referência para SETOR) |

---

## 📋 Entidade: ATENDIMENTO

| Tipo de Atributo | Nome |
|------------------|------|
| **PK** | id_atendimento |
| Simples | data_hora |
| Simples | tipo_atendimento (Consulta, Exame, Cirurgia, Internação...) |
| Simples | prioridade (Normal, Urgente, Emergência) |
| Simples | diagnostico |
| Simples | descricao_procedimento |
| **Derivado (opcional)** | valor_total |
| Simples | forma_pagamento (Convênio, Particular) |
| **FK** | id_paciente |
| **FK** | id_profissional |
| **FK** | id_setor |
| **FK opcional** | id_leito |

---

# 🔗 Relacionamentos do MER

## 1) PACIENTE — ATENDIMENTO
- Um **paciente** pode ter **vários atendimentos**  
- Um **atendimento** pertence a **um paciente**  
**Cardinalidade:** 1:N

---

## 2) PROFISSIONAL — ATENDIMENTO
- Um **profissional** realiza vários atendimentos  
- Um atendimento é feito por **um único profissional**  
**Cardinalidade:** 1:N

---

## 3) SETOR — ATENDIMENTO
- Um **setor** possui vários atendimentos  
- Um atendimento ocorre em **um setor**  
**Cardinalidade:** 1:N

---

## 4) SETOR — LEITO
- Um setor possui vários leitos  
- Cada leito pertence a apenas um setor  
**Cardinalidade:** 1:N

---

## 5) LEITO — ATENDIMENTO
- Um **leito** pode ser usado em vários atendimentos ao longo do tempo  
- Um atendimento pode ter **um leito ou nenhum**  
**Cardinalidade:** 1:N (participação opcional em ATENDIMENTO)

---

# 🎤 Sugestão de Explicação na Prova Oral

> “Eu fiz um MER simples com cinco entidades principais: Paciente, Profissional, Setor, Leito e Atendimento.  
> O Atendimento é a entidade central que liga todas as outras.  
> Usei atributos compostos (endereço), multivalorados (telefones) e derivados (idade) para mostrar variedade.  
> Esse modelo já permite controlar cadastro, leitos, setores, agendamentos e registros básicos do hospital.”

---
