<template>
  <section class="hero shaddow">
    <div class="container">
      <div class="hero-container">
        <div class="hero-content">
          <div class="text-block">
            <h2>Curso CSS</h2>
            <span>Moda Foca</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style>
  @import url('https://fonts.googleapis.com/css?family=Lato:100,300,700');
  .hero {
    /* background: url("~/assets/img/background.png");
    background-size: cover; */
    background: rgb(91, 26, 19);
  }

  .hero .hero-container {
    font-family: 'Lato', sans-serif;
    background: rgb(218, 48, 37);
    margin: 2rem 0;
    border-radius: 1rem;
    padding: .5rem;
  }

  .hero .hero-container .hero-content {
    background: #2a2a2a;
    padding: 3rem 0;
    border-radius: .5rem;
    display: flex;
    flex-direction: column;
  }
  .hero .hero-container .hero-content .text-block {
    margin: auto;
  }
  .hero .hero-container .hero-content h2 {
    text-align: center;
    font-weight: 700;
    font-size: 3rem;
    color: #fff;
    text-shadow: 0 0 .4rem gold;
    margin-bottom: 0;
    letter-spacing: .5rem;
    animation: blink .9s infinite alternate;
  }
  .hero .hero-container .hero-content span {
    font-size: 1.2rem;
    letter-spacing: 1rem;
    color: #fff;
    animation: blink .9s infinite alternate;
    animation-delay: .45s;
    text-align: center;
    display: block;
    margin-top: .4rem;
  }
  @-webkit-keyframes blink {
    from { opacity: 1; }
    to { opacity: 0.3; }
  }
  @media screen and (min-width: 734px) {
    .hero .hero-container {
      padding: 1rem;
    }
    .hero .hero-container .hero-content span {
      /* margin-top: 1rem; */
    }
  }
</style>
