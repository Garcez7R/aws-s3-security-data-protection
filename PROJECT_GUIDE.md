# Guia do Projeto: AWS S3 Security & Data Protection

## 🎯 Visão Geral

Este guia detalha a metodologia, os conceitos técnicos e os passos de implementação utilizados no projeto **AWS S3 Security & Data Protection**. O objetivo é fornecer uma visão clara de como proteger dados sensíveis no Amazon S3, indo além das configurações básicas e focando em governança de nível corporativo.

---

## 🛠️ Metodologia de Segurança

Para este projeto, adotamos a estratégia de **Defesa em Profundidade (Defense in Depth)** aplicada especificamente ao armazenamento de objetos, dividida em 4 camadas principais:

### 1. Camada de Acesso (Identidade e Recurso)
*   **IAM Policies:** Controle de quem pode acessar o serviço S3.
*   **Bucket Policies:** Controle granular no nível do recurso (bucket), permitindo restringir acesso por IP, VPC ou exigir condições específicas (como criptografia).
*   **Block Public Access (BPA):** A "rede de segurança" que impede que qualquer configuração errada torne o bucket público.

### 2. Camada de Proteção de Dados (Criptografia)
*   **Encryption in Transit:** Uso obrigatório de TLS (HTTPS) para todas as comunicações.
*   **Encryption at Rest:** Uso de criptografia do lado do servidor (SSE). Focamos no **SSE-KMS** por oferecer maior controle de auditoria e rotação de chaves.

### 3. Camada de Integridade e Resiliência
*   **Versioning:** Proteção contra sobrescritas e deleções acidentais.
*   **MFA Delete:** Exigência de autenticação de dois fatores para deletar versões de objetos.
*   **Object Lock:** Implementação de modelos WORM (Write Once, Read Many) para conformidade regulatória.

### 4. Camada de Monitoramento e Auditoria
*   **S3 Server Access Logs:** Registro detalhado de todas as requisições feitas ao bucket.
*   **AWS CloudTrail:** Auditoria de eventos de nível de gerenciamento e de dados.
*   **Amazon Macie:** Uso de inteligência artificial para identificar dados sensíveis (CPF, cartões, etc) automaticamente.

---

## 📋 Roteiro de Implementação Simulado

### Passo 1: Configuração do "Perímetro de Segurança"
A primeira ação em qualquer bucket de dados sensíveis é habilitar o **S3 Block Public Access** no nível da conta e do bucket. Isso garante que, mesmo que uma ACL ou política seja configurada incorretamente, os dados permanecerão privados.

### Passo 2: Implementação de Criptografia Obrigatória
Configuramos uma **Bucket Policy** que nega explicitamente qualquer operação `s3:PutObject` que não inclua o cabeçalho de criptografia. Isso força todos os usuários e aplicações a criptografarem os dados antes de entrarem no bucket.

### Passo 3: Gestão de Chaves com KMS
Criamos uma **Customer Managed Key (CMK)** no AWS KMS. Ao contrário da chave padrão da AWS, a CMK nos permite definir quem pode usar a chave para criptografar/descriptografar, criando uma separação de deveres entre o administrador do S3 e o administrador de segurança.

### Passo 4: Configuração de Ciclo de Vida e Retenção
Para dados de backup, implementamos políticas de **Lifecycle** que movem dados para classes de armazenamento mais baratas (como S3 Glacier) após 30 dias e os deletam permanentemente após 7 anos, cumprindo requisitos legais de retenção.

---

## 🔍 Principais Arquivos e Seu Propósito

*   `policies/secure-bucket-policy.json`: Demonstra como usar o efeito `Deny` para forçar o uso de HTTPS e criptografia KMS.
*   `analysis/encryption-analysis.md`: Explica por que escolhemos KMS em vez de outras opções para este cenário.
*   `case/s3-leak-prevention-case.md`: Narra como os controles implementados evitariam um vazamento massivo de dados.

---

## 🎓 Conclusão

A segurança no Amazon S3 não é apenas sobre "clicar em um botão de privado". É um conjunto de controles sobrepostos que garantem que, mesmo em caso de falha em uma camada, os dados permaneçam protegidos. Este projeto serve como um blueprint para profissionais que buscam implementar segurança de dados real em ambientes AWS.

---
**Rafael Garcez** | Escola da Nuvem | Turma BRASAO 227
