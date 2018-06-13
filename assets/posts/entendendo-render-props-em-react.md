
Saudações meu povo. Depois de mais de 2 anos sem escrever um post, finalmente consegui um tempinho, fiz um novo layout para o Blog a nova versão foi escrita em Vue Js com Nuxt ( mas isso é assunto para um novo post ). Hoje vim falar com vocês em específico de uma feature em React que está fazendo muito barulho atualmente ( data de escrita desse post ). E como vi que muitas pessoas ficaram com dúvida de como usar eu resolvi escrever um pouco sobre uma experiência que tive e que talvez ajude alguns de vocês a entender essa feature do React.

Peço mil perdões porque como esse é um post bem específico não vou ficar me atentando a explicar o que é o React ou como funciona, até porquê existem vários tipos de materiais por ai que vão te fazer entender bem essa feature, e partindo do pressuposto que você veio até aqui pelo título do post.

Recentemente estou criando a ementa de um curso que eu e o [Caio Alcantara](https://twitter.com/clucasalcantara) vamos postar pra vocês em breve pela Udemy, e nesse curso vão haver alguns componentes que o aluno ficará como portfolio para serem utilizados em projetos futuros, e me veio na cabeça a ideia de criar um componente `<Fetch>`.

## Entendendo o problema


Não é exatamente um problema, mas uma coisa que estava me incomodando bastante ultimamente no React era o fato de que mesmo trabalhando com componentes, algumas coisas se tornam muito difíceis de serem reaproveitadas, principalmente quando estamos falando de lógicas envolvendo ciclo de vida, ou algo que fuja um pouco da nosso bom e velho render( ) e eu sou um pouco incomodado com diretório de 'utils' dentro de projetos ( as vezes são sim necessários ). E recentemente eu li um post do [Michael Jackson](https://twitter.com/mjackson) que é um dos criadores do react router e foi meu tutor no Nanodegree de React da Udacity ( não me pagaram pra fazer jabá, #thalesMeMandaUmaCamisa ), e ele explicou essa feature do **renderProps** ou também conhecida como **children as a function** que resolvia uma coisa que estava me incomodando e resolvi deixar isso aqui para vocês que tenho certeza que vai ajudar muito, aliás, [clique aqui](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce) para ler o post dele sobre o assunto, mas uma coisa que me chamou atenção é que no post dele, ele colocou um exemplo um pouco abstrato demais e resolvi explicar aqui para vocês com um exemplo mais da vida real pra vocês entenderem o quão poderoso é essa feature. Mas vamos então ao que interessa.

Em quase todos os projetos que fazemos em React precisamos consumir dados de apis de outros locais correto? E para fazer isso temos que criar fetchs em nossos projetos. Existem algumas formas de se fazer fetch com react, mas a lógica é sempre a mesma, dêem uma olhada no exemplo:

```js
import React, { Component } from 'react'

class Fetch extends Component, { Fragment } {
  constructor() {
    super()
    this.state = {
      data: [],
      isLoading: true,
      hasError: false
    }
  }
  componentDidMount() {
    try {
    fetch('https://swapi.co/api/people/1', { method: 'GET' })
    	.then(res => res.json())
      .then(data => this.setState({ data, isLoading: false }))
    }
    catch(e) {
      this.setState({ isLoading: false, hasError: true })
    }
  }
  render() {
    if (hasError) {
      return <h1>Um erro aconteceu e não foi possível fazer o fetch!</h1>
    }
    if (this.state.isLoading) {
      return <h1>Loading...</h1>
    }
    return (
      <Fragment>
        <h1>name: {data.name}</h1>
        <p>cabelo: {data.hair_color}</p>
        <p>Cor da Pele: {data.skin_color}</p>
        <p>Cor do olho: {data.eye_color}</p>
        <p>data de Nascimento: {data.birth_year}</p>
        <p>gênero: {data.gender}</p>
      </Fragment>
    )
  }
}

export default Fetch;

```

Se você observar com atenção vai perceber que:

* O componente inicia em um estado de Loading ou alguma flag que te permita controlar se aquele dado já chegou ou não para o componente.

* O fetch é feito depois que a estrutura do componente já existe e faz um `setState` dentro do `ComponentDidMount` ( maneira recomendada na documentação do React ), depois que esses dados chegam para a aplicação, fazendo a aplicação re-renderizar e trazer finalmente os dados.

Você pode perceber também que o método `fetch()`, recebe dois parâmetros, o primeiro a url, e o segundo recebe um objeto com o método http e algumas configurações opcionais como *header* e *body* por exemplo, no caso do nosso exemplo como estou pegando dados da api do Star Wars que é aberta, ele não me pediu nada disso.

Até aí tudo bem, mas e se eu quisesse criar um componente de fetch no qual eu passasse para ele a url e ele fizesse toda a logica de fetch e eu usasse esse resultado dentro da children do próprio componente? Seria muito f* top não seria?

```js
<Fetch url='https://swapi.co/api/people/1'>
  <h1>name: {data.name}</h1>
</Fetch>
```

Você deve estar dizendo, ah isso é possível de se fazer com HOC, eu não vou entrar nos detalhes de HOC, até porque recentemente o Rafael Maruta escreveu um [post muito legal](https://medium.com/reactbrasil/meu-primeiro-higher-order-component-a376efc654a8) explicando de uma maneira bem simples o que é e como funciona. O que eu posso falar sobre HOC é que não é possível fazê-lo e mesmo se fosse possível seria muito inviável, seriam aqueles tipo de componente que é difícil de dar manutenção até mesmo pela mesma pessoa que fez depois de algum tempo. Mas então qual é a solução que você me propõe Lucas?

## A solução


Utilizando o renderProps podemos resolver isso facilmente através de um conceito conhecido como children as a function, pra isso basta entender que o react nos permite retornar UI através de funções e usar isso a nosso favor, veja como é simples:

```js
import React, { Component } from 'react';

class Fetch extends Component {
  constructor() {
    super()
    this.state = {
      data: [],
      isLoading: true,
      hasError: false
    }
  }
  componentDidMount() {
    try {
      fetch(this.props.url, this.props.configdata)
        .then(res => res.json())
        .then(data => this.setState({ data, isLoading: false }))
    }
    catch(e) {
      this.setState({ isLoading: false, hasError: true })
    }
  }
  render() {
    if (hasError) {
      return <h1>Um erro aconteceu e não foi possível fazer o fetch!</h1>
    }
    if (this.state.isLoading) {
      return <h1>Loading...</h1>
    }
    return (
      <div className='character'>
        {this.props.render(this.state.data)}
      </div>
    )
  }
}

export default Fetch;
```

Note aqui duas diferenças:

1- O método fetch dentro do ComponentDidMount recebe dois parâmetros, o primeiro é o `this.props.url` e o segundo o `this.props.configdata` ambos serão passados para esse componente via props.

2- Dentro do retorno do render estamos usando o `this.props.render` que nesse caso não é o render em si, mas sim os dados que estamos passando via `props` do componente pai para esse componente de Fetch.

Agora vejamos como ficaria o Componente pai utilizando essa proposta.


```js
import React, { Fragment } from 'react';
import Fetch from './Fetch'
import './App.css';

const configFetch = {
  method: 'GET',
  headers: {  } // seus dados de headers para config do fetch SE HOUVER
}

const App = () => {
  return (
    <div className='app'>
      <Fetch url='https://swapi.co/api/people/2' configdata={configFetch} render={(data) => (
        <Fragment>
          <h1>name: {data.name}</h1>
          <p>cabelo: {data.hair_color}</p>
          <p>Cor da Pele: {data.skin_color}</p>
          <p>Cor do olho: {data.eye_color}</p>
          <p>data de Nascimento: {data.birth_year}</p>
          <p>gênero: {data.gender}</p>
        </Fragment>
      )} />
    </div>
  )
}

export default App
````

Note que, passando o render em formato de função, temos o acesso aos parâmetros contidos nessa função, ou seja, conseguimos assim fazer com que um componente se comunique com o outro. O que acontece é que, o react pode receber props de qualquer tipo correto? Seja objeto, string etc, nesse caso ao invés de passarmos uma estrutura de dados convencional, estamos passando definitivamente a própria UI como props se eu não me engano essa estrutura de dados é chamada Symbol me corrija nos comentários (educadamente) se eu estiver errado ok? ;-)

E voilá, agora temos um componente de fetch totamente reutilizável para usar quantas vezes você quiser na sua interface.

![UI Star Wars API](/posts/react/render-props-react/starwars-hansolo-bg_1x.jpg)

Vamos escrever os testes devidos para esse componente e para muitos outros no curso que sairá em breve, não deixe de me seguir, e seguir também o Caio Alcântara nas redes sociais. :-)

E você? Qual uso dessa feature seria legal para implementar em seu workflow? Fiquei curioso agora. Deixe nos comentários aqui abaixo.

