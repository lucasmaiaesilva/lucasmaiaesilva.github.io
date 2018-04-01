<template>
  <section class="hero shaddow">
    <div class="container">
      <div class="led-board">
        <div class="coffee">
          <img src="~/assets/img/coffee.svg" alt="">
        </div>
        
        <div class="content-led">
          <p class="title">Curso completo de CSS</p>
          <p class="description">Pela Udemy - em breve</p>
          <p class="description large-up">Siga em minhas redes sociais para cupons de desconto 😉</p>
        </div>
      </div>
    </div>
  </section>
</template>

<style>
  @import url('https://fonts.googleapis.com/css?family=Lato:100,300,700');
  .hero {
    background: #110C26;
    padding: 3rem 0;
  }
  .led-board {
    background: url("~/assets/img/board.svg");
    background-size: 100% contain;
    background-position-x: center;
    background-repeat: no-repeat;
    height: 13rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .led-board .coffee {
    display: none;
  }

  .led-board .content-led {
    text-align: center;
    color: #fff;
  }

  .led-board .content-led .title {
    font-size: 1.8rem;
  }

  .led-board .content-led .description {
    font-size: 1.2rem;
    margin: .5rem 0;
    letter-spacing: .1rem;
  }

  .led-board .content-led .large-up {
    display: none;
  }

  @media screen and (min-width: 734px) {
    .led-board {
      height: 18rem;
    }
    .led-board .content-led .large-up {
      max-width: 18rem;
      margin: 0 auto;
      margin-top: 1rem;
      font-weight: 300;
      display: block;
      font-size: .9rem;
    }
  }

  @media screen and (min-width: 1024px) {
    .led-board .coffee {
      display: block;
      margin-right: 1rem;
    }
    .coffee img {
      width: 10rem;
      height: auto;
    }
    .led-board {
      height: 22rem;
    }
  }

</style>
