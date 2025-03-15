<template>
  <span id="time">Time:  0</span>
  <div class="main-container">
    <div class="game-container">
    </div>
  </div>
  <button @click="setCell(questions[0]);Start()">Set Cell</button>
</template>

<script>
export default {
  name: 'App',
  setup() {
    const questions = [
      [[7, 1, 14, 6],
      [4, 2, 8, 10],
      [3, 5, 12, 9],
      [13, 15, 11, 0]],
    ];
    let question, id;
    let seconds = 0;
    const isGameOver = () => {
      let count = 1;
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          if (i==3&&j==3) {
            if (question[i][j] !== 0) {
              return false;
            }
            break;
          }
          if (question[i][j] !== count) {
            return false;
          }
          count++;
        }
      }
      return true;
    }
    const Start = () => {
      seconds = 0;
      clearInterval(id);
      id=setInterval(() => {
        seconds++;
        document.getElementById('time').innerText = `Time: ${seconds}`;
      }, 1000);
    }
    const GameOver = () => {
      clearInterval(id);
      const overlay = document.createElement('div');
      overlay.classList.add('overlay');
      overlay.addEventListener('click', () => {
        overlay.remove();
        document.querySelector('.message').classList.add("up");
        setTimeout(() => {document.querySelector('.message').remove();}, 500);
      });
      document.body.appendChild(overlay);
      const message = document.createElement('div');
      message.classList.add('message');
      message.innerText = 'Game Over\nTime: ' + seconds;
      document.body.appendChild(message);
      let ex=-1, ey=-1;
      for (let i = 0; i < 4; i++) {
        if (question[i].indexOf(0) !== -1) {
          ex = i;
          ey = question[i].indexOf(0);
          break;
        } 
      }
      const em = document.querySelector(`.c${ex + 1}-${ey + 1}`);
      em.classList.remove('empty');
      em.classList.add('empt');
    }
    const moveCell = (i, j) => {
      let ex=-1, ey=-1;
      for (let i = 0; i < 4; i++) {
        if (question[i].indexOf(0) !== -1 && document.querySelector(`.c${i + 1}-${question[i].indexOf(0) + 1}`).classList.contains('empty')) {
          ex = i;
          ey = question[i].indexOf(0);
          break;
        } 
      }
      if ((i===ex && Math.abs(j-ey)===1) || (j===ey && Math.abs(i-ex)===1)) {
        question[ex][ey] = question[i][j];
        question[i][j] = 0;
        setCell(question);
        if (isGameOver()) {
          GameOver();
        }
      }
    }
    const setCell = (q) => {
      const gameContainer = document.querySelector('.game-container');
      gameContainer.innerHTML = '';
      question = q;
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          const cell = document.createElement('div');
          cell.classList.add('cell');
          cell.classList.add(`c${i + 1}-${j + 1}`);
          if (q[i][j] === 0) {
            cell.classList.add('empty');
          }
          if (q[i][j]!==0) cell.onclick = () => moveCell(i, j);
          cell.innerText = q[i][j]===0 ? '' : q[i][j];
          gameContainer.appendChild(cell);
        }
      }
    }
    return {
      questions,
      setCell,
      seconds,
      Start,
    }
  },
};
</script>

<style>
:root {
  background-color: rgb(255, 239, 220);
}

@font-face {
  font-family: 'ClearSans';
  src: url('./assets/fonts/ClearSans-Bold.woff');
}

.main-container {
  border: none;
  width: 500px;
  height: 500px;
  border-radius: 5px;
  background-color: rgb(208, 135, 82);
}

.game-container {
  width: 480px;
  height: 480px;
  padding: 10px;
  display: grid;
  grid-template-rows: repeat(4, 1fr);
  grid-template-columns: repeat(4, 1fr);
}

.cell {
  margin: 10px;
  color: #fff;
  cursor: pointer;
  font-size: 50px;
  padding-top: 9px;
  border-radius: 4px;
  transition: all .3s;
  font-family: "ClearSans";
  background-color: rgb(166, 102, 56);
}

.empty.cell , .empt.cell{
  cursor: default!important;
  background-color: transparent!important;
}
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 9;
}
.message {
  z-index: 99;
  border-radius: 5px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  width: 200px;
  animation: down .5s backwards;
  font-weight: bold;
}

.message.up {
  animation: up .5s backwards;
}

@keyframes up {
  0% {
    top: 50%;
  }
  20% {
    top: 55%;
  }
  100% {
    top: -50%;
  }
}

button {
  background-color: royalblue;
  border-radius: 3px;
  border: none;
  outline: none;
  margin: 15px;
  width: 200px;
  color: #fff;
}

button:hover, button:active {
  outline: none!important;
}

@keyframes down {
  0% {
    top: -50%;
  }
  80% {
    top: 55%;
  }
  100% {
    top: 50%;
  }
}

#time {
  width: 500px;
  text-align: left;
  font-size: xx-large;
  font-weight: 600;
}

.c1-1 {
  grid-row: 1;
  grid-column: 1;
}
.c1-2 {
  grid-row: 1;
  grid-column: 2;
}
.c1-3 {
  grid-row: 1;
  grid-column: 3;
}
.c1-4 {
  grid-row: 1;
  grid-column: 4;
}
.c2-1 {
  grid-row: 2;
  grid-column: 1;
}
.c2-2 {
  grid-row: 2;
  grid-column: 2;
}
.c2-3 {
  grid-row: 2;
  grid-column: 3;
}
.c2-4 {
  grid-row: 2;
  grid-column: 4;
}
.c3-1 {
  grid-row: 3;
  grid-column: 1;
}
.c3-2 {
  grid-row: 3;
  grid-column: 2;
}
.c3-3 {
  grid-row: 3;
  grid-column: 3;
}
.c3-4 {
  grid-row: 3;
  grid-column: 4;
}
.c4-1 {
  grid-row: 4;
  grid-column: 1;
}
.c4-2 {
  grid-row: 4;
  grid-column: 2;
}
.c4-3 {
  grid-row: 4;
  grid-column: 3;
}
.c4-4 {
  grid-row: 4;
  grid-column: 4;
}
</style>
