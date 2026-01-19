# Estudo de Caso: Prevenção de Vazamento de Dados no Amazon S3

## 📋 Resumo Executivo

Este estudo de caso detalha a estratégia de segurança implementada pela **TechNova Solutions** para proteger seu repositório central de backups e documentos de clientes. Após um incidente anterior focado em identidades (IAM), o foco mudou para a **proteção direta do recurso de dados**, garantindo que, mesmo que uma identidade seja comprometida, os dados permaneçam inacessíveis ou protegidos por camadas adicionais.

---

## 1. O Problema: O Risco da Exposição Silenciosa

A TechNova Solutions identificou que possuía mais de **500TB** de dados sensíveis (backups de banco de dados, contratos em PDF e logs de transações) armazenados em diversos buckets S3. Uma auditoria interna revelou três riscos críticos:
1.  **Configurações de ACL Legadas:** Alguns buckets antigos ainda possuíam permissões de "Leitura Pública" ativas.
2.  **Falta de Criptografia Padronizada:** Cerca de 30% dos objetos não estavam criptografados em repouso.
3.  **Risco de Deleção Acidental:** Não havia versionamento ativo, tornando os dados vulneráveis a erros humanos ou Ransomware.

---

## 2. A Solução: Arquitetura de "Cofre de Dados"

A equipe de segurança, implementou uma arquitetura baseada em 5 pilares de proteção:

### A. Perímetro de Acesso Público Zero
Foi ativado o **S3 Block Public Access** em nível de conta. Isso agiu como um "disjuntor", desativando instantaneamente qualquer permissão pública existente e impedindo a criação de novas, independentemente de políticas de bucket ou ACLs.

### B. Criptografia Forçada com SSE-KMS
Em vez de apenas habilitar a criptografia padrão, foi implementada uma **Bucket Policy** que nega qualquer upload (`s3:PutObject`) que não utilize criptografia KMS. 
*   **Resultado:** 100% dos novos dados são criptografados automaticamente com chaves gerenciadas pelo time de segurança.

### C. Imutabilidade com Object Lock
Para os backups financeiros, foi ativado o **S3 Object Lock** no modo de conformidade (*Compliance Mode*). 
*   **Resultado:** Nem mesmo o usuário root da conta pode deletar ou modificar esses arquivos durante o período de retenção de 5 anos, garantindo proteção total contra Ransomware.

### D. Monitoramento Inteligente com Amazon Macie
O **Amazon Macie** foi configurado para realizar varreduras periódicas nos buckets.
*   **Resultado:** O Macie identificou automaticamente arquivos que continham CPFs e números de cartões de crédito que estavam em pastas incorretas, permitindo a remediação imediata.

---

## 3. Simulação de Ataque e Eficácia dos Controles

### Cenário: Comprometimento de Credenciais de Desenvolvedor
Um atacante obteve as *Access Keys* de um desenvolvedor que possuía a política `AmazonS3FullAccess`.

| Tentativa do Atacante | Controle de Segurança Ativo | Resultado |
| :--- | :--- | :--- |
| Tentar tornar o bucket público para exfiltração rápida. | **S3 Block Public Access** | **NEGADO.** O controle de nível de conta impede a alteração para público. |
| Tentar ler um backup financeiro sensível. | **KMS Key Policy** | **NEGADO.** O desenvolvedor tem acesso ao S3, mas não tem permissão `kms:Decrypt` na chave específica. |
| Tentar deletar todos os arquivos (Ransomware). | **Object Lock & Versioning** | **FALHOU.** Os arquivos bloqueados não puderam ser deletados. Versões anteriores foram mantidas. |
| Tentar baixar dados via HTTP para interceptação. | **Bucket Policy (SecureTransport)** | **NEGADO.** A política exige HTTPS obrigatoriamente. |

---

## 4. Lições Aprendidas e Resultados

A implementação destes controles resultou em:
*   **Conformidade com a LGPD:** Garantia técnica de que dados pessoais estão protegidos e auditados.
*   **Resiliência Operacional:** Proteção contra erros humanos que anteriormente causavam perda de dados.
*   **Cultura de Segurança:** O time de desenvolvimento passou a integrar cabeçalhos de criptografia em todas as integrações de API.

---

## 5. Conclusão

A segurança do Amazon S3 não deve depender apenas de "não ser público". A verdadeira proteção de dados em nuvem exige uma abordagem multi-camada onde a criptografia, a imutabilidade e o monitoramento trabalham juntos para criar um ambiente onde o dado é seguro por design.

---
**Rafael Garcez** | Escola da Nuvem | Turma BRASAO 227
