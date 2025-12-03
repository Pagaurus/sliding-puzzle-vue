<script setup>
  import { reactive, h, provide, useTemplateRef, ref } from 'vue';

  import Square from './components/Square.vue';
  import Piece from './components/Piece.vue';

  let win = false;

  let pieces = [];

  for(let i = 0; i < 8; i++){
    pieces.push(
      h(Piece, {
        image: '/src/images/' + i +'.jpeg'
      })
    );
  }

  pieces.push(
    h(Piece, {
      image: null
    })
  )

  let position = reactive(randomNums(9, 1000));

  let exchanges = [];

  function randomNums(len, shuffAmnt){
    let numArr = [];

    for(let i = 0; i < len; i++){
      numArr.push(i);
    }

    for(let i = 0; i < shuffAmnt; i++){
      let shuffNum = numArr.splice(
        Math.floor(Math.random() * len),
      1)[0];

      numArr.push(shuffNum);
    }

    return numArr;
  }

  let board = [];

  let highlighted = null;

  function getCoords(index){
    return {
      x: index % 3,
      y: Math.floor(index / 3)
    }
  }

  function runGame(selected){
    if(highlighted != null){
      if(selected != highlighted){
        let oldCoords = getCoords(highlighted);
        let newCoords = getCoords(selected);

        if(
            (position[highlighted] == 8 || position[selected] == 8)
          &&
            (
              (Math.abs(newCoords.x - oldCoords.x) == 1 && newCoords.y == oldCoords.y)
              ^
              (Math.abs(newCoords.y - oldCoords.y) == 1 && newCoords.x == oldCoords.x)
            )
        ){
          exchanges = [];

          exchanges[selected] = oldCoords;
          exchanges[highlighted] = newCoords;

          let dPiece = position[highlighted];

          position[highlighted] = position[selected];

          position[selected] = dPiece;
        }
      }

      board[highlighted].unhighlight();
    }

    if(highlighted != selected){
      highlighted = selected;
      board[highlighted].highlight();
    }else{
      highlighted = null;
    }
  }
</script>

<template>

  <div class="grid grid-cols-3 grid-flow-row gap-0.5 w-115 m-auto" ref="grid">

    <Square
      v-for="i in 9"
      ref="board"
      :piece="pieces[position[i - 1]]"
      :direction="exchanges[i - 1] ? {
        x: exchanges[i - 1].x - getCoords(i - 1).x,
        y: exchanges[i - 1].y - getCoords(i - 1).y
      } : {x: 0, y: 0}"
      @click="runGame(i - 1);"
      key="i - 1"
    />
  </div>

</template>

<style>
  @import "tailwindcss";
</style>
