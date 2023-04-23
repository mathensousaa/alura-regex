# Expressões regulares: capturando textos de forma mágica

# 1. Começando com Regex

## Começando a aprender Regex com Javascript

<aside>
🔤 Regex são um ***padrão de definir uma determinada cadeia de caracteres*** dentro de um texto maior

</aside>

Por exemplo ao procurar arquivos no prompt de comandos do Linux

```powershell
ls | grep -p ".*png"
```

- Certos comandos server para filtrar conteúdos específicos, como no exemplo da imagem abaixo:

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled.png)

- Como funcionam as expressões regulares:
    - O **pattern** é só a regra
    - Uma expressão regular sozinha é apenas uma string. É preciso ter um software para interpretar a regex e aplicá-la no alvo. Esse software é o **Regex Engine**
    - E assim teremos um **match**
    
    ![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%201.png)
    

## O nosso primeiro problema

CSV - comma separated value.

O nosso objetivo é tentar encontrar um padrão em cada linha:

```
João Fulano,123.456.789-00,21 de Maio de 1993,(21) 3079-9987,Rua do Ouvidor,50,20040-030,Rio de Janeiro
Maria Fulana, 98765432100,11 de Abril de 1995,(11) 933339871,Rua Vergueiro,3185,04101-300,São Paulo
denise teste, 987.654.321.00,28 de Dezembro de 1991,(31)45562712,SCS Qd. 8 Bl. B-50,11,70333-900,Rio Grande
```

- **`\d`** - dígitos
- meta-char - alguns caracteres que possuem um significado especial para o regex engine. Especial significa que o regex engine não interpreta o valor literal e sim diferente.
- **`.**` - Representa qualquer caractere especial
- **`\.`** - Representa o valor literal do .
- **`{999} ou *`** - quantifier - caractere especial para definir quantidade de caracteres

## **Mão na massa - Encontrando o CNPJ**

```jsx
\d{3}\.\d{3}\.\d{3}-\d{2} //regex para encontrar cpf 123.456.789-00
```

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%202.png)

## **Mão na massa - Encontrando o IP**

- Regex para IP levando em conta que cada grupo pode ter de 1 a 3 dígitos
    - `126.1.112.34`
    - `128.126.12.244`
    - `192.168.0.34`

```jsx
\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}
```

<aside>
🌐 Um IP tem quatro grupos de no mínimo um e máximo três números. Repare que estamos escapando o ponto (.) entre os números, que são blocos de dígitos \d entre 1 e 3 caracteres {1,3}

</aside>

## **Mão na massa - Encontrando o CEP**

- definir a regex para encontrar o CEP dentro de uma linha no nosso CSV

```
João Fulano,123.456.789-00,21 de Maio de 1993,(21) 3079-9987,Rua do Ouvidor,50,20040-030,Rio de Janeiro
```

```jsx
\d{5}-\d{3}
```

## **Mão na massa - Buscando o telefone**

- Qual padrão podemos utilizar para encontrar o número telefônico? Por exemplo: **(21) 3216-2345**

```jsx
\(\d{2}\) \d{4,5}-\d{4}
```

## **Para que servem Regex?**

- Em resumo os Regex servem para:
    - Extrair seções específicas de um arquivo de texto
    - Validação de formatação de, por exemplo, e-mail ou telefone
    - Análise de arquivos de texto e extração de dados para, por exemplo, gravar no banco de dados
    - Substituir o dos valores de um texto para limpar, reformatar ou alterar o conteúdo

# 2. Classes com caracteres

## Entendendo Classes de Caracteres

- **`?**` - Caractere não obrigatório
- **`{0,1}`** - Também torna o caractere não obrigatório
- **`[-.]** ou **[0-9]`** - Definir classe de caracteres

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%203.png)

```jsx
\d{3}\.?\d{3}\.?\d{3}[-.]?\d{2}
```

## **Mãos na massa: Ajudando Alura**

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%204.png)

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%205.png)

## Praticando classes e quantifier

- **`\s`** - classe para espaços, sejam eles espaço ou tab
- **`{1,} ou +`** - uma ou mais vezes
- **`?`** - zero ou uma vezes
- **`*`** - zero ou mais vezes
- **`{n}`** - exatamente n vezes
- **`{n,}`** - no mínimo n vezes
- **`{n,m}`** - no mínimo n+1 vezes, no máximo m vezes.
- **`\w`** - significa *word char* e é uma atalho para `[A-Za-z0-9]`.

```
denise teste, 987.654.32100,28      de Março de 1991,(31)45562712,SCS Qd. 8 Bl. B-50,11,70333-900,Rio Grande
```

```jsx
[0-3]?\d\s+de\s+[A-Z][a-zç]{3,8}\s+de\s+[12]\d{3} // expressão regular para filtrar datas
```

![Untitled](Expresso%CC%83es%20regulares%20capturando%20textos%20de%20forma%20m%20007730573ba9406d875f6d1aab590292/Untitled%206.png)

## Trabalhando com horários

```
19h32min16s.
```

```cpp
\d{2}h\d{2}min\d{2}s // regex para filtrar esse padrão de tempo
```

## **Mão na massa: Reconhecendo a placa de um veículo**

```
KMG-8089
```

```jsx
[A-Z]{3}-\d{4}
```

## **Mão na massa: expressão regular a favor dos alunos!**

Ajude Gilberto e, claro, seus alunos, separando do arquivo CSV os **nomes e as notas dos** alunos que tiraram de `7.2` a `7.9` para que o professor "camarada" possa aprová-los!

```
9.8 - Robson, 7.1 - Teresa, 4.5 - Armênio, 6.5 - Zulu, 7.7 - Stefania, 7.8 - João, 5.0 - Romeu, 7.2 - Pompilho, 3.1 - Reinaldo, 7.3 - Bernadete, 4.7 - Cinério
```

```jsx
7\.[2-9]\s+-\s+[^,]+
```

## **Mão na massa: Uma expressão regular incorreta pode prejudicar alguém**

```
10 - Bruce, 9.5 - Miranda, 7.9    - Bob, 10 - Zimbabue, 7.5 - Bety
```

```jsx
[7]\.[5-9]\s+-\s+\w+
```

## **Mão na massa: Separando joio do trigo**

Escreva uma expressão regular que faça *match* apenas com as palavras GARROTE, SERROTE e ROTEIRO. Não esqueça de usar nossa ferramenta para testar nossas expressões regulares.

```
BALEIRO GARROTE SERROTE GOLEIRO ROTEIRO
```

```jsx
[A-Z]*ROTE[A-Z]*
ou
[A-Z]*ROT[A-Z]+
```

## **Opcional: Validando o usuário no serviço Rest**

O `username` precisa ser da seguinte forma:

- O limite é de 10 caracteres;
- O primeiro caractere deve ser uma letra do alfabeto, não pode ser um número;
- A partir do segundo caractere podemos ter letras maiúsculas, minúsculas e números;

Como deve ficar a anotação `@Pattern` com uma expressão regular com essas características?

```java
public class User {
    @Pattern(regexp = "???")
    @NotEmpty
    private String username;

}
```

```cpp
[a-zA-Z][a-zA-Z\d]{0,9}
```

## **Para saber mais: Melhorando a legibilidade**

Na aula criamos um pequeno "monstro" para definir a expressão da data. Como poderíamos deixar a expressão mais fácil de entender?

```jsx
[0-3]?\d\s+de\s+[A-Z][a-zç]{3,8}\s+de\s+[12]\d{3}
```

```jsx
var DIA  = "[0-3]?\d"; 
var DE = "\s+de\s+";
var MES  = "[A-Za-z][a-zç]{3,8}";
var ANO  = "[12]\d{3}";

var stringRegex = DIA + DE +  MES + DE + ANO;

var objetoRegex  = new RegExp(stringRegex, 'g');
```

# Encontrando a posição certa com âncoras

# Trabalhando com grupos

# Ganancioso ou preguiçoso

# Usando regex nas diversas linguagens
