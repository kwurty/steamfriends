<template>
  <div class="game">
      <div class="title">{{game.name}}</div>
      <div class="image"> <img :src="`http://media.steampowered.com/steamcommunity/public/images/apps/${game.appid}/${game.img_logo_url}.jpg`"></div>
  </div>
</template>

<script>
export default {
    props: {
        game: []
    }
}
</script>

<style scoped>
    .game {
        height: 169px;
        background: var(--turquoise);
        display: grid;
        flex-direction: column;
        flex: 1;
    }
    .game div {
        height: 100%;
        min-height: 100%;
        width: 200px;
        display: flex;
        flex-direction: column;
    }
    
    .game .title {
        height: auto;
        background-color: var(--blue);
        color: var(--white);
        font-weight: 700;
    }
    .game img{

    }
</style>