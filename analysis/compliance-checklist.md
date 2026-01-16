# Checklist de Conformidade: Segurança de Dados S3

Este checklist foi desenhado para auxiliar em auditorias rápidas de segurança em ambientes AWS, garantindo que os controles fundamentais de proteção de dados estejam ativos.

---

## 🛡️ Camada de Acesso
- [ ] **Block Public Access:** Habilitado no nível da conta?
- [ ] **Block Public Access:** Habilitado no nível do bucket?
- [ ] **Bucket Policy:** Existe uma política que nega acesso HTTP (não-TLS)?
- [ ] **IAM Roles:** As aplicações usam roles específicas em vez de Access Keys de usuários?
- [ ] **ACLs:** O uso de ACLs foi desabilitado (Bucket owner enforced)?

## 🔐 Camada de Proteção (Criptografia)
- [ ] **Default Encryption:** A criptografia padrão está habilitada no bucket?
- [ ] **Tipo de SSE:** Está sendo usado SSE-KMS para dados sensíveis?
- [ ] **KMS Key Policy:** A política da chave restringe quem pode descriptografar os dados?
- [ ] **Enforcement:** Existe uma Bucket Policy que nega uploads sem criptografia?

## 📈 Camada de Integridade
- [ ] **Versioning:** O versionamento está habilitado?
- [ ] **Object Lock:** Para dados regulatórios, o Object Lock está ativo?
- [ ] **Lifecycle:** Existem regras para mover dados antigos para classes de arquivo (Glacier)?
- [ ] **MFA Delete:** Está configurado para buckets de backup críticos?

## 🔍 Camada de Monitoramento
- [ ] **Server Access Logs:** Estão sendo enviados para um bucket de logs centralizado?
- [ ] **CloudTrail:** Os "Data Events" estão sendo registrados para o bucket?
- [ ] **Amazon Macie:** Foi executado um scan para identificar dados sensíveis (PII)?
- [ ] **Inventory:** O S3 Inventory está configurado para relatórios periódicos de objetos?

---

## 📋 Resumo de Conformidade

| Item | Status Sugerido | Impacto se Ausente |
| :--- | :--- | :--- |
| Block Public Access | **Obrigatório** | Vazamento de dados em massa |
| Encryption (SSE-KMS) | **Crítico** | Exposição de dados em caso de acesso físico/lógico |
| Versioning | **Alto** | Perda definitiva de dados por erro ou ataque |
| Access Logging | **Médio** | Impossibilidade de realizar perícia forense |

---
**Rafael Garcez** | Escola da Nuvem | Turma BRASAO 227
