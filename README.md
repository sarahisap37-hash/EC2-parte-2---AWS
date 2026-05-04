#  AWS Lambda 

Este projeto automatiza a interrupção de instâncias **Amazon EC2** utilizando uma função **AWS Lambda** escrita em Python e agendada pelo **Amazon EventBridge**.

---

##  Passo 1: Criar a Função Lambda
1. No **Console AWS**, pesquise por **Lambda**.
2. Clique em **Criar função** e selecione **Criar do zero**.
3. Configure os detalhes básicos:
   - **Nome da função:** `myStopinator`
   - **Runtime:** `Python 3.11`
4. Em **Permissões**, selecione **Usar uma função existente** e escolha `myStopinatorRole`.
5. Clique em **Criar função**.

---

##  Passo 2: Configurar o Gatilho (EventBridge)
1. No painel da função, clique em **Adicionar gatilho**.
2. Selecione **EventBridge (CloudWatch Events)** no menu suspenso.
3. Escolha **Criar uma nova regra**:
   - **Nome da regra:** `everyMinute`
   - **Tipo:** Expressão de programação.
   - **Expressão:** `rate(1 minute)` 
     *(A função será executada a cada 1 minuto).*
4. Clique em **Adicionar**.

---

##  Passo 3: Implementar o Código
1. Na aba **Código**, abra o arquivo `lambda_function.py`.
2. Substitua o código existente pelo bloco abaixo:

```python
import boto3

# Configurações de destino
region = 'SUA_REGIAO_AQUI'        # Ex: 'us-east-1'
instances = ['ID_DA_INSTANCIA']   # Ex: 'i-0abcd1234efgh5678'

ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('Instâncias interrompidas: ' + str(instances))

