# Relatório de Auditoria de Segurança - FastByte Delivery

## PARTE 1: Mapeamento da Tríade CIA

Abaixo está a análise dos pontos críticos identificados no novo microsserviço de gorjetas:

| Ponto | Pilar da Tríade CIA Quebrado | Risco Real (Impacto Prático) |
| :--- | :--- | :--- |
| **Ponto 1** | **Confidencialidade** | A falta de criptografia (HTTP) permite que atacantes na rede realizem *sniffing* e interceptem dados sensíveis de cartão de crédito. |
| **Ponto 2** | **Integridade** | A ausência de validação de input permite que usuários mal-intencionados insiram valores negativos para subtrair fundos ou caracteres que corrompam o banco de dados. |
| **Ponto 3** | **Disponibilidade** | Sem *rate limit*, o serviço está vulnerável a ataques de negação de serviço (DoS), podendo ficar indisponível por sobrecarga deliberada. |

---

## PARTE 2: O Resgate com Shift-Left

O cenário apresentado demonstra que a segurança está sendo tratada como uma etapa final isolada, o que é um risco para a operação da FastByte.

### Aplicação da Cultura Shift-Left na Esteira CI/CD
Para corrigir o problema do Ponto 4, a segurança deve ser integrada desde o início do ciclo de vida:

1.  **Mudança na Esteira CI/CD:** A pipeline deve atuar como um *gate* de qualidade, bloqueando automaticamente qualquer deploy que contenha falhas de segurança detectadas.
2.  **Prática 1: Análise Estática de Código (SAST):** Implementação de ferramentas de análise automática na pipeline para detectar vulnerabilidades (como falta de validação de input) antes da build.
3.  **Prática 2: Testes Automatizados de Segurança:** Rotinas de testes que validem requisitos de segurança (ex: tentativas de injetar valores inválidos) para garantir que o código seja testado antes de chegar à produção.