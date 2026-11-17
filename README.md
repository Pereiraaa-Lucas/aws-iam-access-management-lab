# 🛡️ Introdução ao AWS Identity and Access Management (IAM)

Laboratório prático focado na implementação de controle de acesso baseado em funções (**RBAC**), fortalecimento de credenciais e validação de permissões na **AWS**.

---

## 🎯 Objetivos do Laboratório
- Configurar e aplicar uma **política de senhas personalizada** no nível da conta AWS.
- Explorar usuários e grupos de usuários pré-criados no **AWS IAM**.
- Analisar a estrutura de **Políticas Gerenciadas pela AWS** (*AWS Managed Policies*) e **Políticas Incorporadas** (*Customer Inline Policies*).
- Atribuir usuários a grupos de acordo com o **Princípio do Privilégio Mínimo** (*Least Privilege*).
- Validar na prática o isolamento de permissões entre os serviços **Amazon S3** e **Amazon EC2**.

---

## 🏗️ Cenário e Matriz de Permissões (RBAC)

A arquitetura de acesso foi estruturada dividindo responsabilidades operacionais entre três perfis distintos:

| Usuário IAM | Grupo Atribuído | Política Anexada | Tipo de Política | Escopo de Permissões |
| :--- | :--- | :--- | :--- | :--- |
| `user-1` | `S3-Support` | `AmazonS3ReadOnlyAccess` | Gerenciada pela AWS | Listar e visualizar buckets e objetos no Amazon S3. |
| `user-2` | `EC2-Support` | `AmazonEC2ReadOnlyAccess` | Gerenciada pela AWS | Listar e descrever instâncias EC2, ELB e CloudWatch (somente leitura). |
| `user-3` | `EC2-Admin` | `EC2-Admin-Policy` | Incorporada (*Inline*) | Descrever, iniciar (`StartInstances`) e interromper (`StopInstances`) instâncias EC2. |

---

## ⚙️ Etapas Executadas

### 1. Fortalecimento da Política de Senhas da Conta
Nas configurações da conta IAM (*Account settings*), a política padrão foi atualizada com os seguintes requisitos de segurança:
- **Comprimento mínimo:** `10 caracteres`.
- **Complexidade:** Exigência de letras maiúsculas, minúsculas, números e caracteres não alfanuméricos.
- **Ciclo de vida:** Expiração a cada `90 dias` e bloqueio de reutilização das últimas `5 senhas`.
- Permissão para os próprios usuários alterarem suas senhas sem depender de redefinição do administrador.

### 2. Inspeção de Políticas IAM
Análise da anatomia das declarações (`Statement`) em JSON compostas por `Effect`, `Action` e `Resource`, diferenciando políticas reutilizáveis (gerenciadas) de políticas pontuais (incorporadas).

### 3. Associação de Usuários aos Grupos
Inclusão de `user-1`, `user-2` e `user-3` em seus respectivos grupos funcionais, garantindo que as permissões sejam herdadas de forma centralizada e escalável.

### 4. Testes de Validação de Acesso
Acesso via URL de login específico da conta IAM em navegação privada para comprovar a aplicação das regras:
- **`user-1`:** Listou buckets do S3 com sucesso; recebeu `Access Denied` ao tentar visualizar instâncias no EC2.
- **`user-2`:** Visualizou instâncias do EC2 em execução; recebeu erro de autorização ao tentar interromper (*Stop*) uma instância e ao listar buckets no S3.
- **`user-3`:** Visualizou e executou com sucesso o desligamento (*Stopping*) da instância EC2.

---

## 💡 Principais Aprendizados
- **Gerenciamento via Grupos:** Atribuir políticas a grupos em vez de usuários individuais reduz falhas operacionais e facilita auditorias de segurança.
- **Princípio do Privilégio Mínimo:** Cada identidade recebe estritamente as ações necessárias para sua função, mitigando riscos de alterações acidentais ou movimentação lateral.

---

**Tecnologias e Conceitos:** `AWS IAM` • `Amazon EC2` • `Amazon S3` • `JSON` • `RBAC` • `Cloud Security`
