# Guia de Boas Práticas: Segurança no Amazon S3

## 🚀 Introdução

O Amazon S3 é um dos serviços mais utilizados da AWS, mas também um dos mais visados por atacantes devido a configurações incorretas. Este guia consolida as melhores práticas aplicadas neste projeto para garantir a máxima proteção dos dados.

---

## 1. Bloqueio de Acesso Público (Block Public Access)
Esta é a configuração mais importante. O **S3 Block Public Access** deve ser habilitado no nível da conta para evitar que qualquer bucket seja exposto acidentalmente.
*   **Ação:** Habilitar as 4 opções de bloqueio no console ou via CLI.
*   **Benefício:** Previne vazamentos causados por erros humanos em políticas de bucket ou ACLs.

## 2. Princípio do Menor Privilégio (Least Privilege)
Nunca use o usuário root ou políticas `AdministratorAccess` para aplicações que apenas precisam ler ou escrever em um bucket específico.
*   **Ação:** Crie IAM Roles específicas para cada aplicação, limitando o acesso apenas aos ARNs dos buckets necessários.
*   **Benefício:** Se uma aplicação for comprometida, o atacante terá acesso limitado apenas àquele bucket, não a toda a conta.

## 3. Criptografia Obrigatória
Dados sensíveis nunca devem ser armazenados em texto claro.
*   **Ação:** Use Bucket Policies para negar uploads que não utilizem criptografia (SSE-KMS).
*   **Benefício:** Garante que 100% dos dados no bucket estejam protegidos em repouso.

## 4. Versionamento e MFA Delete
Proteja seus dados contra erros operacionais ou ataques de Ransomware.
*   **Ação:** Habilite o versionamento. Para buckets críticos, configure o MFA Delete.
*   **Benefício:** Permite recuperar versões anteriores de arquivos deletados ou modificados maliciosamente.

## 5. Auditoria e Monitoramento
Você não pode proteger o que não consegue ver.
*   **Ação:** Habilite o **S3 Server Access Logs** e o **CloudTrail Data Events**.
*   **Benefício:** Fornece um rastro forense completo em caso de investigação de incidentes.

## 6. Uso de VPC Endpoints
Evite que seus dados trafeguem pela internet pública.
*   **Ação:** Configure um Gateway VPC Endpoint para o S3.
*   **Benefício:** O tráfego entre suas instâncias EC2 e o S3 permanece dentro da rede privada da AWS, aumentando a segurança e reduzindo custos de transferência.

---

## 🏁 Conclusão

Seguir estas práticas transforma o S3 de um simples "depósito de arquivos" em um cofre de dados altamente seguro e auditável.

---
**Rafael Garcez** | Escola da Nuvem | Turma BRASAO 227
