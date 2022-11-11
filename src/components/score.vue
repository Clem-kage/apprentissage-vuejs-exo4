<template>
  <div>
    <section class="score" >
      Player <span>{{this.userScore}}</span> x 
     <span>{{this.computerScore}}</span> Computer
    </section>
  </div>
</template>

<script>
export default {
    name:"scoreBoard",
    props: {
        userScore: Number,
        computerScore: Number
    }
}
</script>

<style>

section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;
}

section.score span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
  }

</style>