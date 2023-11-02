# Teste Diamond Solutions

![texto alt](https://media.licdn.com/dms/image/C4E16AQHHZj8VFWrtEQ/profile-displaybackgroundimage-shrink_350_1400/0/1659031081520?e=1704326400&v=beta&t=saVEXXEDZ04vhq0EYrbE5a8Xk27LUoNCbBR_KMxcxBU)


Este repositório contém as soluções em SQL e Python, para o teste para a vaga de Estágio da Empresa Diamond Solutions.

## Pré-requisitos

- SQL e Python.

## Passo a passo

**SQL** - Foi Criado um banco de Dados no Postgres, para a criação das tabelas e resolução das questões.
**PYTHON** - Foi utilizada a ferramenta VSCODE para resolução da questão 3.

## Resolução questões 1 e 2:

Execute as seguintes consultas para verificar a estrutura e as colunas das tabelas envolvidas nas questões:


### Passo a passo na resolução das questões.

## QUESTÃO 1

Um aparelho faz gravações da quantidade de luz recebida em uma área por períodos de tempo. Também existe um indicador que é incrementado em cada mudança de temperatura. Para reduzir o tamanho da mensagem que é enviada, o aparelho compacta os registros próximos com o mesmo valor e acrescenta a quantidade de registros compactados. Construa uma consulta para solucionar a compressao e mostrar a quantidade de luz e agrupamento de registros.

**1- CRIAÇÃO DO BANCO DE DADOS**

Execute a seguinte consulta para obter o resultado:

```sql
CREATE DATABASE database_teste_estagio;

```

**2 - CRIAÇÃO DAS TABELAS**

Execute a seguinte consulta para obter o resultado:

### TABELA REGISTROS

```sql
CREATE TABLE tabela_registros (
  id integer PRIMARY KEY,
  quantidade_luz integer,
  mark integer
);

```

**3- INSERINDO DADOS**

Execute a seguinte consulta para obter o resultado:

### TABELA REGISTROS

```sql
INSERT INTO tabela_registros (id, quantidade_luz, mark) VALUES
(1, 45, 1),
(2, 45, 1),
(3, 45, 1),
(4, 64, 2),
(5, 64, 2),
(6, 64, 2),
(7, 64, 2),
(8, 60, 3),
(9, 60, 3),
(10, 60, 3),
(11, 62, 4),
(12, 62, 4),
(13, 66, 5),
(14, 66, 5),
(15, 66, 5);

```

**4 - EFETUANDO O SELECT SOLICITADO**

```sql
SELECT
  quantidade_luz,
  COUNT(*) AS agrupamento_mark
FROM tabela_registros
GROUP BY quantidade_luz
ORDER BY quantidade_luz;

```

### EXPLICAÇÃO

**SELECT** quantidade_luz, COUNT(*) AS agrupamento: Esta parte da consulta define as colunas selecionada no resultado. Aqui, estamos selecionando duas colunas. A primeira coluna é quantidade_luz, que é a quantidade de luz que queremos agrupar. A segunda coluna é COUNT(*), que é uma função de agregação que conta o número de registros em cada grupo e renomeamos o resultado para agrupamento_mark.

**FROM** tabela_registros: Aqui, especificamos a tabela de onde os dados serão consultados.

**GROUP BY** quantidade_luz: Esta cláusula agrupa os registros com base nos valores da coluna quantidade_luz. Isso significa que todos os registros com o mesmo valor de quantidade_luz serão agrupados juntos.

**ORDER BY** quantidade_luz: Esta cláusula ordena o resultado com base na coluna quantidade_luz em ordem crescente. Isso significa que os resultados serão exibidos em ordem crescente de valores de quantidade_luz.

Portanto, a consulta conta quantos registros existem para cada valor único de quantidade_luz na tabela tabela_registros e retorna os resultados em ordem crescente com a quantidade de luz e a contagem de registros agrupados.

### RESULTADO

![resultado 1](https://github.com/NayaraWakewski/teste_diamond_solutions/assets/79403619/549905f3-a31e-4e01-8cb1-a983d3be8660)






## QUESTÃO 2

O setor de armazenagem precisa realizar o inventário de quantos produtos existem em estoque, e você tem a oportunidade de ajudá-los. Seu trabalho é exibir o nome e a quantidade de produtos de cada categoria.

**1 - CRIAÇÃO DAS TABELAS**

Execute a seguinte consulta para obter o resultado:

### TABELA CATEGORIAS

```sql
CREATE TABLE tabela_categorias (
  id_categoria INTEGER PRIMARY KEY,
  nome VARCHAR(100)
);

```

### TABELA PRODUTO

```sql
CREATE TABLE tabela_produto (
  id_produto INTEGER PRIMARY KEY,
  nome VARCHAR(100),
  quantidade INTEGER,
  preco NUMERIC(10, 2),
  id_categoria INTEGER REFERENCES tabela_categorias(id_categoria)
);

```

**2- INSERINDO DADOS**

Execute a seguinte consulta para obter o resultado:

### TABELA CATEGORIA

```sql
INSERT INTO tabela_categorias (id_categoria, nome) VALUES
(1, 'madeira'),
(2, 'luxo'),
(3, 'vintage'),
(4, 'moderno'),
(5, 'super luxo');

```

### TABELA PRODUTO

```sql
INSERT INTO tabela_produto (id_produto, nome, quantidade, preco, id_categoria) VALUES
(1, 'Armário de duas portas', 100, 800.00, 1),
(2, 'Mesa de jantar', 1000, 560.00, 3),
(3, 'Porta toalha', 10000, 25.50, 4),
(4, 'Mesa de computador', 350, 320.50, 2),
(5, 'Cadeira', 3000, 210.64, 4),
(6, 'Cama de solteiro', 750, 460.00, 1);

```

**3 - EFETUANDO O SELECT SOLICITADO**

```sql
SELECT c.nome AS nome_categoria, SUM(p.quantidade) AS quantidade_total_de_produtos
FROM tabela_categorias c
LEFT JOIN tabela_produto p ON c.id_categoria = p.id_categoria
GROUP BY c.nome
HAVING SUM(p.quantidade) IS NOT NULL
ORDER BY quantidade_total_de_produtos DESC;

```

### EXPLICAÇÃO

Claro, vou explicar a consulta SQL passo a passo:

**SELECT** c.nome AS nome_categoria, SUM(p.quantidade) AS quantidade_total_de_produtos`: Esta parte da consulta seleciona o nome da categoria da tabela "tabela_categorias" e calcula a soma da coluna "quantidade" da tabela "tabela_produto" para cada categoria. O resultado é nomeado como "nome_categoria" e "quantidade_total_de_produtos" para facilitar a leitura dos resultados.

**FROM** categorias c`: Nesta parte, definimos um alias "c" para a tabela "tabela_categorias" para simplificar a referência a ela na consulta.

**LEFT JOIN** produto p ON c.id = p.id_categoria`: Aqui, estamos fazendo um LEFT JOIN entre a tabela "tabela_categorias" (alias "c") e a tabela "tabela_produto" (alias "p"). Estamos usando a coluna "id_categoria" da tabela "tabela_categorias" e a coluna "id_categoria" da tabela "tabela_produto" como critério de junção. O LEFT JOIN garante que todas as categorias sejam incluídas na consulta, independentemente de terem produtos associados a elas.

**GROUP BY** c.nome`: Com a cláusula GROUP BY, estamos agrupando os resultados pelo nome da categoria. Isso significa que a soma será calculada para cada categoria separadamente.

**HAVING SUM** (p.quantidade) **IS NOT NULL**`: O HAVING é usado para filtrar os grupos após a agregação. Neste caso, estamos removendo os grupos nos quais a soma da coluna "quantidade" é nula (ou seja, não há produtos cadastrados a essa categoria). Portanto, estamos excluindo categorias sem produtos associados.

**ORDER BY** quantidade_total_de_produtos **DESC**: Finalmente, estamos ordenando os resultados pela coluna "quantidade_total_de_produtos" em ordem decrescente (DESC). Isso significa que as categorias com a maior quantidade total de produtos serão listadas primeiro.

Dessa forma, a consulta retorna o nome da categoria e a quantidade total de produtos em cada categoria, excluindo categorias com quantidade de produtos nulos e ordenando o resultado pela quantidade total em ordem decrescente.

### RESULTADO

![resultado 2](https://github.com/NayaraWakewski/teste_diamond_solutions/assets/79403619/fbb22b51-c4ae-4b71-9950-7dafd2ccb8a1)

### OBSERVAÇÃO

Poderiamos ter utilizado o INNER JOIN, ele já excluiria da consulta as categorias sem produtos cadastrados. Optei por mostrar as duas formas, que trazem o mesmo resultado!

O SELECT ficaria assim:

```sql
SELECT c.nome AS nome_categoria, SUM(p.quantidade) AS quantidade_total_de_produtos
FROM categorias c
INNER JOIN produto p ON c.id = p.id_categoria
GROUP BY c.nome
ORDER BY quantidade_total_de_produtos DESC;
```

## QUESTÃO 3

Na avaliação da performance de resolução de tickets, pretende-se, de uma lista de filas e utilizadores, seguida da duração de resolução (em minutos) por utilizador, obter os
seus tempos médios de resolução.
Estrutura da entrada:
>_ primeira linha com numero de utilizadores por fila
>_ n linhas com par “utilizador fila” (tantos pares quanto o valor anterior)
>_ numero de tickets resolvidos
>_ n linhas com par “utilizador duracao” (tantos pares quanto o valor anterior)
>
A saída da aplicação deve ser feito para o console, usando a função print() onde se pode ler a duração média (em minutos) por utilizador, assim como por fila, arredondados
para 1 casa decimal.
Como extra, pretende-se obter o resultado ordenado da maior para a menor duração média (ignorando a ordem alfabética). O formato da saída deve ser nos pares “utilizador duração” e “àrea duração”.

Exemplo:
if __name__ == '__main__':
num_utilizadores_areas = int(input())

**Nessa questão, me baseei pelas aulas que tive na Infinity School em Programação com Python, a grade de lá não era exclusivamente PYTHON para Data Science, então vimos função em PYTHON e a lógica também SEM USAR função. Utilizei de fontes de pesquisa para conseguir resolver a questão, com base no que aprendi . Troquei o nome FILA por ÁREA, acho que FILA confundi muito o usuário**


## CÓDIGO SEM USAR FUNÇÃO.

```python

# Dicionários para armazenar os tempos de resolução por utilizador e por área
tempos_utilizador = {}
tempos_area = {}

# Solicitar o número de utilizadores por área
num_usuarios_por_area = int(input("Digite o número de utilizadores por área: "))

# Preencher os dicionários com os tempos de resolução (fazendo um loop)
for _ in range(num_usuarios_por_area):
    usuario, area = input("Digite o utilizador e a área separados por um espaço: ").split()
    tempos_utilizador.setdefault(usuario, [])
    tempos_area.setdefault(area, [])

# Solicitar o número de tickets resolvidos
num_tickets_resolvidos = int(input("Digite o número de tickets resolvidos: "))

# Preencher os dicionários com os tempos de resolução por utilizador e área (fazendo um loop)
for _ in range(num_tickets_resolvidos):
    usuario, duracao = input("Digite o utilizador e a duração separados por um espaço: ").split()
    tempos_utilizador.setdefault(usuario, []).append(int(duracao))
    area = input("Digite a área: ")
    tempos_area.setdefault(area, []).append(int(duracao))

# Cálculo das médias e impressão
print("\nMédia de duração por utilizador:")
for usuario, tempos in tempos_utilizador.items():
    media = sum(tempos) / len(tempos) if tempos else 0
    print(f"{usuario} {media:.1f}")

print("\nMédia de duração por área:")
for area, tempos in tempos_area.items():
    media = sum(tempos) / len(tempos) if tempos else 0
    print(f"{area} {media:.1f}")

# Ordenar os resultados da maior para a menor duração média (por utilizador)
ordenado_por_duracao = sorted(tempos_utilizador.items(), key=lambda x: -sum(x[1]) / len(x[1]) if x[1] else 0)
print("\nResultado ordenado por duração média (maior para menor):")
for usuario, tempos in ordenado_por_duracao: (fazendo um loop)
    media = sum(tempos) / len(tempos) if tempos else 0
    print(f"{usuario} {media:.1f}")

```

## INPUT

1 - Irá preencher com a quantidade de utilizadores, que são os atendentes, usuários... No exemplo do teste incluimos 3.

2 - Irá preencher com o nome do utilizador e área (fila), no exemplo: ADALBERTO SSI e assim com todos da lista.

3 - Irá preencher com a quantidade de chamados atendidos, no exemplo são 8 no total.

4 - Irá prencher com o nome do Utilizador seguido do tempo, no exemplo: ADALBERTO 15.

5- Irá preencher com a área daquele Utilizador que você respondeu na pergunta anterior, no caso do Adalberto é SSI.

E assim irá fazer para os demais 7 chamados.
Ao finalizar no console aparecerá o resultado.

## CÓDIGO SEM USAR FUNÇÃO (NESSE PRECISEI USAR PESQUISA PARA LEMBRAR COMO FAZER FUNÇÃO).

```python
# Função para calcular a média de uma lista de valores.
def calcular_media(valores):
    return sum(valores) / len(valores) if valores else 0

#Define uma função chamada calcular_media que calcula a média de uma lista de valores. A função retorna a soma dos valores dividida pelo número de valores, arredondada para uma casa decimal. Se a lista estiver vazia, retorna 0.

# Função para ler uma lista de valores separados por espaços
def ler_lista_valores(mensagem):
    valores = input(mensagem).split()
    return [int(valor) for valor in valores]

#Define uma função chamada ler_lista_valores que recebe uma mensagem como entrada, lê uma linha da entrada padrão (o que o usuário digitar), divide essa linha em valores separados por espaços, converte cada valor para um número inteiro e retorna a lista de números.

# Função para processar as entradas e calcular as médias
def calcular_medias():
    num_usuarios_por_area = int(input("Digite o número de utilizadores por área: "))
    tempos_utilizador = {}
    tempos_area = {}

#Solicita ao usuário o número de utilizadores por área.
#Inicializa dois dicionários, tempos_utilizador e tempos_area, para armazenar informações sobre os tempos de resolução por utilizador e por área.

    for _ in range(num_usuarios_por_area):
        usuario, area = input("Digite o utilizador e a área separados por um espaço: ").split()
        tempos_utilizador[usuario] = []
        tempos_area[area] = []

#Em um loop, solicita ao usuário que insira pares "utilizador e área", adicionando o utilizador à lista de utilizadores em tempos_utilizador e a área à lista de áreas em tempos_area.

    num_tickets_resolvidos = int(input("Digite o número de tickets resolvidos: "))

#Solicita ao usuário o número de tickets resolvidos.


    for _ in range(num_tickets_resolvidos):
        usuario, duracao = input("Digite o utilizador e a duração separados por um espaço: ").split()
        tempos_utilizador[usuario].append(int(duracao))
        area = input("Digite a área: ")
        tempos_area[area].append(int(duracao))

#Em outro loop, solicita ao usuário que insira pares "utilizador e duração" e adiciona a duração à lista de tempos do utilizador correspondente em tempos_utilizador. Também solicita ao usuário a área correspondente para adicionar a duração à lista de tempos da área correspondente em tempos_area.

    # Cálculo das médias e impressão
    print("\nMédia de duração por utilizador:")
    for usuario, tempos in tempos_utilizador.items():
        media = calcular_media(tempos)
        print(f"{usuario} {media:.1f}")

    print("\nMédia de duração por área:")
    for area, tempos in tempos_area.items():
        media = calcular_media(tempos)
        print(f"{area} {media:.1f}")

#Calcula a média de duração por utilizador e por área usando a função calcular_media e imprime os resultados.

    # Ordenar os resultados da maior para a menor duração média (por utilizador)

    ordenado_por_duracao = sorted(tempos_utilizador.items(), key=lambda x: -calcular_media(x[1]))
    print("\nResultado ordenado por duração média (maior para menor):")
    for usuario, tempos in ordenado_por_duracao:
        media = calcular_media(tempos)
        print(f"{usuario} {media:.1f}")

# Chamada da função principal
calcular_medias()

#Chama a função principal calcular_medias para executar o processo completo.

```

### RESULTADO

![Captura de tela 2023-11-02 160325](https://github.com/NayaraWakewski/teste_diamond_solutions/assets/79403619/6293175e-688d-4b96-acf4-a11b7d120e16)


## 🎁 Expressões de gratidão

* Obrigada pela oportunidade de participar do processo seletivo 📢;

---
⌨️ com ❤️ por [Nayara Vakevskii](https://github.com/NayaraWakewski) 😊

