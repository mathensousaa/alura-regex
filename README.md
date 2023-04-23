# JavaScript para Web: Crie páginas dinâmicas

# Conhecendo o Javascript

## Clicando no botão

A tag audio do HTML precisa ter o atributo controls para ser exibida a barra de controles.

## onclick

O evento onclick executa certo comportamento usando JavaScript quando um botão é clicado

```html
<button onclick="alert('Pom')" class="tecla tecla_pom">Pom</button>
```

## Conectar JS com HTML

Escrever Javascript dentro do HTML não é adequado, pois é muito mais demorada a manutenção. Porém pode ser usado para fins de teste.

```html
<script src="./main.js"></script>
```

```jsx
alert('Olá mundo!'); // o ponto e virgula no Javascript é opcional, porém é recomendado usar para evitar alguns erros no futuro.
```

## Buscar um elemento

É parecido com o que já fazemos com o CSS.

### Query Selector

O alert é uma funcionalidade que está em um escopo maior que é uma janela. Já o query selector é uma funcionalidade que precisa ter um escopo, que é onde você quer que o Javascript encontre o elemento com o seletor que voce definiu.

Para isso usamos o document que é uma palavra reservada referenciando todo o documento HTML.

O “.” serve para acessar esse escopo que voce selecionou que no caso é o document e usar as funções que aquele escopo tem.

```jsx
document.querySelector('')
```

selecionar o elemento HTML `input`do tipo `tel`

```jsx
document.querySelector(‘input[type=tel]’)
```

# Funções

## Play no JS

Ao escrever . após o objeto selecionado será mostrada todas as funções que podem ser usadas.

```jsx
document.querySelector('#som_tecla_pom').play()
```

## O que é uma função?

### Local correto do Script

Quando o navegador “abre” o arquivo HTML, se o script está no head, ele será lido antes do body e portanto não vai reconhecer certos objetos que estão no body.

### Política dos navegadores

Os navegadores tem uma política padrão de não permitir que seja executado sons assim que se entra em uma página.

Por isso acontece um erro quando tentamos dar play em um som sem que o usuário clique em algo.

<aside>
💡 Quando um código precisa ser chamado só quando necessário precisamos de uma função para isso.

</aside>

```jsx
// criando uma função para reproduzir o som de pom. sem retorno e sem parametros)
function playSoundPom () {
    document.querySelector('#som_tecla_pom').play();
}
```

## Clique no botão

podemos passar a função no HTML, porém não é algo recomendado

```jsx
<button onclick="playSoundPom()" class="tecla tecla_pom">Pom</button>
```

Podemos usar o Javascript

```jsx
document.querySelector('.tecla_pom').onclick = playSoundPom;
```

Não devemos chamar  a função playSoundPom, mas sim atribuir o onclick a playSoundPom

# Listas

## Listas de elementos

Podemos copiar TODAS as funções e chamadas de funções para CADA UM dos sons, porém se existissem 1000 botões teríamos que criar uma função para cada um deles, o que seria nada semântico e de difícil manutenção.

### querySelectorAll

Ao utilizarmos o querySelectorAll ao invés de capturarmos um elemento de cada vez, conseguimos pegar todos os elementos de uma só vez, isso facilita a manipulação, reutilização e manutenção do código.

Ao invés de selecionar um único objeto precisamos selecionar vários.

```jsx
document.querySelectorAll('.tecla')
//NodeList(9) [button.tecla.tecla_pom, button.tecla.tecla_clap, button.tecla.tecla_tim, button.tecla.tecla_puff, button.tecla.tecla_splash, button.tecla.tecla_toim, button.tecla.tecla_psh, button.tecla.tecla_tic, button.tecla.tecla_tom]
```

## Referências

Precisamos deixar nosso código legível e pra isso usamos referencias, que são criadas com base no valor que elas vão guardar: por exemplo, se nosso objeto tiver o valor constante e não sera alterado ao longo do script, caso seja alterado seja variável declarar ela como variável. 

Devemos ter por costume colocar nomes sugestivos

```jsx
const keysList = document.querySelectorAll('.tecla');
```

Isso deixa o código mais legível.

## Conhecendo listas

Acessando individualmente um elemento de uma lista:

```jsx
keyList[index]
```

# Iterando em listas

## Utilizando loops para iterar em listas

### While

Podemos utilizar o loop for para iterar por cada item da lista e adicionar um event listener para cada um deles.

```jsx
while (count < keysList.length) {
    keysList[count].onclick = playSoundPom;
    
    count++;
}
```

Neste exemplo, estamos adicionando um onclick para cada item da lista keysList, que executa a função playSoundPom quando o botão é clicado.

## Funções com parâmetros

Devemos criar funções genéricas que servem para varias coisas evitando ser especificar para um so objeto.

Os parâmetros são os nomes que damos a valores que uma função pode receber em sua chamada, que podem ou não ter um valor padrão. Os parâmetros são valores que ficam disponíveis apenas no corpo da funcao.

Por exemplo em uma função calculaMedia(), pode-se ter como parâmetros notaA e notaB, que são valores utilizados dentro dessa funcao.

No nosso caso usamos o parâmetro para não precisar repetir a função para cada um dos sons a serem executados

```jsx
function playSound (audioId) {
    document.querySelector(audioId).play();
}
```

## Funções anônimas

Na Função playSound temos que passar um parâmetro, porem ao atribuir onclick a função playSound não podemos chama-la pois nao podemos passar parâmetro ao atribuir, então como vamos passar o parâmetro para a função?

Para isso usamos funções anônimas:

```jsx
keysList[count].onclick = function () {
        playSound('#som_tecla_pom');
    };
```

Porém o ID da tecla ainda está fixo.

## Textos dinâmicos

na função anônima para reproduzir o som, o som ainda está estático

```jsx
keysList[count].onclick = () => {
        playSound('#som_tecla_pom');
    };
```

Para ficar de mais fácil entendimento e sem repetição de código desnecessário podemos criar uma constante para cada tecla da lista de teclas:

```jsx
const key = keysList[count]; //constante para cada tecla da lista
const instrument = key.classList[1]; //constante para a classe de cada uma das teclas
```

Dessa forma o código completo fica assim: 

```jsx
//função que reproduz o som que tem a ID passada como parâmetro
playSound = (audioId) => {
    document.querySelector(audioId).play();
}

//criando um array com todos os itens que tem a classe .tecla
const keysList = document.querySelectorAll('.tecla');

//loop que itera por todos os elementos em keysList.
for (let count = 0; count < keysList.length; count++) {
	  const key = keysList[count]; //key recebe o elemento da lista na posição atual
    const instrument = key.classList[1]; //instrument recebe a segunda classe do elemento atual
    const idAudio = `#som_${instrument}`; //idAudio é definido como uma string que será usada para selecionar um elemento de audio no DOM

		
    key.onclick = () => playSound(idAudio); //definido um evento de clique para cada elemento key, o manipulador de eventos é uma funçãp anônima que chama a função play sound com o argumento idAudio
}
```

# Eventos e lógicas

## Eventos do teclado

Ao tentar reproduzir o som usando o teclado, ao apertar espaço o sistema faz a animação de click, porém quando usamos enter não funciona, para isso precisamos usar o evento chamado **onkeydown**

```jsx
key.onkeydown = () => key.classList.add('ativa');
```

## **Adicionando e removendo classe**

```jsx
key.onkeydown = () => key.classList.add('ativa');
key.onkeyup = () => key.classList.remove('ativa');
```

O problema dessa função é que ele faz isso com todas as teclas, então quando se pressiona a tecla tab a tecla de som fica ativa

## Condições no código

Se a barra de espaço ou a tecla enter for pressionada executar a função de manipulação do evento.

<aside>
🎫 Quando é passado um evento para uma função, o primeiro parâmetro é um objeto com vários detalhes do evento.

</aside>

```jsx
key.onkeydown = (event) => {
        console.log(event);
        key.classList.add('ativa');
    }
```

Dessa forma podemos fazer isso:

```jsx
key.onkeydown = (event) => {
        event.code === 'Enter' || event.code === 'Space' ? key.classList.add('ativa') : '';
    }
```

## Operador lógico

```jsx
key.onkeydown = (event) => event.code === 'Enter' || event.code === 'Space' ? key.classList.add('ativa') : '';
```

## Mais condições

Caso o usuário chame a função playSound com um parametro inválido precisamos exibir um erro:

```jsx
playSound = (audioSelector) => {
    const element = document.querySelector(audioSelector);

    element === null ? console.log('Elemento não encontrado') : element.play();
}
```

## Melhorando o código

```jsx
playSound = (audioSelector) => {
    const element = document.querySelector(audioSelector);

    element != null && element.localName === 'audio' ? element.play() : console.log('Elemento não encontrado');
}
```
