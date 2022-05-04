
### Random_transactions

##### Sobre

<p>O projeto é um Mock de uma API de transações</p>
<p>As transações são geradas com dados aleatórios, com seguinte formato</p>

```
[
  {
     "descricao": "string(10, 60)"
     "data": "long(timestamp)"
     "valor": "integer(-9.999.999, 9.999.999)"
     "duplicated": "boolean"
  }  
]

```
#### Endpoint
<p>Por pandrão, o server roda na porta 8080, então para acessar deve se ir em:</p>
<p>localhost:8080/{id}/transacao/{ano}/{mes}</p>

<p>/{id}/transacao/{ano}/{mes} -GET Retorna uma lista com transações aleatorios para cada mes ano e id </p>

- @params id(number)
- @params ano(number)
- @params mes(number)

#### Regras de negócio

- dado um `conjunto de dados`, formado por um id de usuário, um ano e um mês, retornar -se uma lista de transações
- a lista de transações tem`quantidade variável entre os meses`
- o id de usuário é um `número inteiro` de 1.000 a 100.000.000
- cada transação tem uma `descrição aleatória legível` no formato string
- caso o conjunto de transações tenha duas ou mais transações com a `mesma descrição, data e valor`, todas, menos uma, `tem duplicated true`
- cada descrição tem no mínimo `10 caracteres`
- cada descrição não supera `60 caracteres`
- cada transação deve tem um `valor aleatório`
- o valor da transação é representado por um `número inteiro`
- o valor da transação tem seus `2 últimos dígitos representando os centavos`
- um valor de `8989` representa, portanto, `R$ 89,89`
- o valor da transação esta entre `-9.999.999 e 9.999.999`, inclusive
- cada transação tem o `timestamp de uma data aleatória` em formato `long`
- a data aleatória esta `dentro do range de ano e mês` dados
- dado dois `conjuntos de dados` iguais, as respostas são as mesmas, priorizando um execução deterministica


<p>Para a geração dos dados aleatórios, foi utilizado o
  <a href="https://github.com/DiUS/java-faker">JavaFaker</a></p>
  
  ```kotlin
/*
Exemplo do JavaFaker
*/

//Classe que define uma transação
data class Transaction(
        val id: Int,
        val descricao: String,
        var duplicated: Boolean,
        val valor: Int,
        val timestamp: Long
)
// Instancia do JavaFaker
val faker = Faker(Random())
//Mock da transação
val transaction = Transaction(
        1, // O id não é gerado de forma randomica, ja que vem via endpoint
        faker.regexify("([bcdfghjklmnpqrstvxyz][aeiou]){10,120}"), //gera uma descrição aleatorio de acordo com a regex passada
        true, // a descrição não é gerada de forma randomica, ja q é passada via endpoint
        faker.number().numberBetween(-9999999, 9999999), // gera um conjunto de numero seguindo  o intervalo determinado
        faker.number().numberBetween(1577836800, 1609459199).toLong() // gera um conjunto de numero seguindo o intervalo
                                                                      //neste caso o intervalo equivale a 30 dia  

)

  ```
  
  


##### Pré Requisitos para rodar o projeto
- Java 8 JVM - pelo menos a versão 8u171 .
- Gradlee 6.6.1
- Docker (Opcional)

#### Executando o projeto localmente

<p>Baixe o repositório:</p>

```
https://github.com/danilocutrim/Back-end-Engineer-GB.git
```
<p>Entre no diretorio do projeto </p>

```
cd Back-end-Engineer-GB
```
<p>Compile o projeto </p>

```
./gradlew build
```

<p>Execute o projeto </p>

```
java -jar build/libs/transactions-0.0.1-SNAPSHOT.jar 
```

#### Executando o projeto com o docker

<p>Construa a imagem </p>

```
docker build . -t transaction-api
```
<p>Execute a imagem</p>

```
docker run -p 8080:8080 transaction-api
```

<p>Configurando monitoramento da Aplicação com prometheus</p>

<p>Referencias:
 https://stackabuse.com/monitoring-spring-boot-apps-with-micrometer-prometheus-and-grafana/</p>

```
docker pull prom/prometheus

```

```
docker run -d -p 9090:9090 -v /home/danilo/Workspace/Back-end-Engineer-GB/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

```
#!/bin/sh
LOCAL=$('origin/HEAD')
for br in `git branch -r`; do
    echo $br
    if [[git branch = $LOCAL]];
    then
      echo "testeeeee"
   fi
    git checkout $br
    for dir in `git ls-files */`; do
      echo ${br#*origin/},${dir} >> log.csv
    done
    #git ls-files
done

```
docker pull docker pull grafana/grafana

```
```
docker pull grafana/grafana

```
```
sudo docker run -d -p 3000:3000 grafana/grafana

```
