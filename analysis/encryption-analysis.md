# Análise Técnica: Métodos de Criptografia no Amazon S3

## 📋 Introdução

A criptografia é a última linha de defesa para os dados. No Amazon S3, existem diversas formas de implementar a criptografia do lado do servidor (SSE). Esta análise compara as opções disponíveis e justifica a escolha do **SSE-KMS** para o projeto da TechNova Solutions.

---

## 🔍 Comparativo de Métodos SSE (Server-Side Encryption)

| Característica | SSE-S3 | SSE-KMS | SSE-C |
| :--- | :--- | :--- | :--- |
| **Gestão da Chave** | Totalmente pela AWS | Pelo usuário via AWS KMS | Pelo usuário (fora da AWS) |
| **Custo** | Gratuito (incluído no S3) | Custo por chave e por uso | Gratuito (mas complexo de gerir) |
| **Auditoria** | Logs básicos do S3 | Logs detalhados no CloudTrail | Logs básicos do S3 |
| **Rotação de Chave** | Automática (gerida pela AWS) | Configurável pelo usuário | Manual pelo usuário |
| **Controle de Acesso** | Apenas via S3/IAM | Via S3/IAM + KMS Key Policy | Via S3/IAM + Posse da Chave |

---

## 🎯 Por que escolhemos SSE-KMS?

Para a **TechNova Solutions**, a escolha do **SSE-KMS (Key Management Service)** foi baseada em três pilares fundamentais:

### 1. Separação de Deveres (Segregation of Duties)
Com o SSE-KMS, um usuário pode ter permissão no IAM para ler o objeto no S3 (`s3:GetObject`), mas se ele não tiver permissão na política da chave KMS (`kms:Decrypt`), ele **não conseguirá ler o conteúdo do arquivo**. Isso cria uma camada dupla de segurança.

### 2. Auditoria Detalhada
Cada vez que um arquivo é criptografado ou descriptografado usando uma chave KMS, um evento é registrado no **AWS CloudTrail**. Isso permite que o time de segurança saiba exatamente quem acessou qual dado e quando, algo essencial para conformidade com a **LGPD**.

### 3. Controle de Ciclo de Vida da Chave
Diferente do SSE-S3, onde a chave é genérica para o serviço, no SSE-KMS podemos desabilitar ou deletar a chave em caso de suspeita de comprometimento, tornando todos os dados criptografados com ela instantaneamente ilegíveis.

---

## 🛡️ Criptografia em Trânsito (TLS)

Além da criptografia em repouso, este projeto força o uso de **TLS 1.2+** através de Bucket Policies. Isso impede ataques de *Man-in-the-Middle* (MitM), garantindo que os dados nunca trafeguem de forma limpa pela rede.

---

## ✅ Conclusão

Embora o SSE-S3 seja suficiente para dados genéricos, o **SSE-KMS** é o padrão ouro para dados sensíveis, backups financeiros e informações de clientes (PII). Sua implementação demonstra um nível de maturidade em segurança que é altamente valorizado em ambientes corporativos.

---
**Rafael Garcez** | Escola da Nuvem | Turma BRASAO 227
