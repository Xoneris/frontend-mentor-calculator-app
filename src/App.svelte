<script lang="ts">

  let display:Array<string|number> = [];
  let showResult:boolean = false;
  let result:any;

  

  function addToDisplay(character:string|number) {

    showResult = false

    if (typeof character === "string") {
      // First element in the Array cannot be a string (+,-,*,/ or .)
      if (display.length === 0){
        return
      }
      // Cannot input a string if previous array element is also a string (+,-,*,/ or .)
      if (typeof display[display.length-1] === "string") {
        return
      }
      if (character === ".") {
        // Cannot input a "." if the last string in the array is also a "."
        let found:boolean = false;
        for (let i = display.length; i >= 0; i--){
          if (typeof display[i] === "string") {
            if (display[i] === ".") {
              found = true;
              break;
            } else {
              break;
            }
          }
        }
        if(found === true){
          return
        }
      }
    } else if (typeof character === "number") {
      // Cannot enter a number if the two previous array elements are a math operator (+,-,*,/) followed by a 0.
      if (["+","-","*","/"].includes(display[display.length-2] as string) && display[display.length-1] === 0){
        return
      }
    }
      checkFontSize()
      // Add the element to the Array and reassign so Svelte rerenders.
      display.push(character);
      display = [...display];

  }

  function removeLastElement() {
    display.pop();
    display = [...display];
  }

  function calculate() {
    
    if (typeof display[display.length-1] === "string"){
      display.pop();
    }

    let temp:Array<string|number> = []
    let tempNumber:string = ""

    for(let i = 0; i < display.length; i++){

      if (typeof display[i] === "number" || display[i] === ".") {
        tempNumber += display[i]; 
      } else {
        if (tempNumber.length > 0) {
          temp.push(Number(tempNumber))
          temp.push(display[i])
          tempNumber = ""
        }
      }
    }
    if (tempNumber.length > 0) {
      temp.push(Number(tempNumber))
    }

    for(let j = 0; j < temp.length; j++) {

      let numberOne = 0
      let numberTwo = 0

      if(temp[j] === "*" || temp[j] === "/"){

        let tempResult:number = 0
        numberOne = Number(temp[j-1])
        numberTwo = Number(temp[j+1])

        if (temp[j] === "*"){
          tempResult = numberOne * numberTwo
        }
        if (temp[j] === "/"){
          tempResult = numberOne / numberTwo
        } 
        temp.splice(j-1,3,tempResult)
        j -= 1;
      }
    }

    for(let j = 0; j < temp.length; j++) {

      let numberOne = 0
      let numberTwo = 0

      if(temp[j] === "+" || temp[j] === "-"){

        let tempResult:number = 0
        numberOne = Number(temp[j-1])
        numberTwo = Number(temp[j+1])

        if (temp[j] === "+"){
          tempResult = numberOne + numberTwo
        }
        if (temp[j] === "-"){
          tempResult = numberOne - numberTwo
        } 
        temp.splice(j-1,3,tempResult)
        j -= 1;
      }
    }

    showResult = true;
    result = temp;
    display = [];
  }

  function reset() {
    display = [];
    showResult = false;
    const displayElement:HTMLElement = document.getElementById('display') as HTMLElement
    displayElement.style.fontSize = 48 + 'px';
  }

  function checkFontSize() {
    const displayElement:HTMLElement = document.getElementById('display') as HTMLElement
    const displayWidth = displayElement.offsetWidth
    const displayTextElement:HTMLElement = document.getElementById('displayText') as HTMLAnchorElement
    const textWidth = displayTextElement.offsetWidth

    let fontSize = parseInt(window.getComputedStyle(displayElement).fontSize);

    if (textWidth+64 > displayWidth) {
      fontSize -= 3;
      displayElement.style.fontSize = fontSize + 'px';
    }

  }

</script>

<main class="w-screen h-screen flex justify-center items-center bg-[#3B4664]">
  
  <div class="flex flex-col gap-4 w-[539px]">
    
    <div class="w-full h-32 p-8 bg-[#181F32] text-white rounded-lg font-bold flex justify-end items-center" id="display">
      <p id="displayText">
      {#if showResult === true}
        {result}
      {:else}
        {#each display as char}
          {char}
        {/each}
      {/if}
      </p>
    </div>

    <div class="bg-[#252D44] p-8 rounded-lg flex flex-wrap justify-center gap-6">
      <button class="button" on:click={() => addToDisplay(7)}>7</button>
      <button class="button" on:click={() => addToDisplay(8)}>8</button>
      <button class="button" on:click={() => addToDisplay(9)}>9</button>
      <button class="w-24 h-16 bg-[#647299] hover:bg-[#A2B3E1] rounded-lg text-2xl font-bold shadow-[0_3px_0_0_#414E71] text-white" on:click={() => removeLastElement()}>DEL</button>
      <button class="button" on:click={() => addToDisplay(4)}>4</button>
      <button class="button" on:click={() => addToDisplay(5)}>5</button>
      <button class="button" on:click={() => addToDisplay(6)}>6</button>
      <button class="button" on:click={() => addToDisplay("+")}>+</button>
      <button class="button" on:click={() => addToDisplay(1)}>1</button>
      <button class="button" on:click={() => addToDisplay(2)}>2</button>
      <button class="button" on:click={() => addToDisplay(3)}>3</button>
      <button class="button" on:click={() => addToDisplay("-")}>-</button>
      <button class="button" on:click={() => addToDisplay(".")}>.</button>
      <button class="button" on:click={() => addToDisplay(0)}>0</button>
      <button class="button" on:click={() => addToDisplay("/")}>/</button>
      <button class="button" on:click={() => addToDisplay("*")}>x</button>  
      <button class="w-[218px] h-16 bg-[#647299] hover:bg-[#A2B3E1] rounded-lg text-2xl font-bold shadow-[0_3px_0_0_#414E71] text-white" on:click={() => reset()}>RESET</button>
      <button class="w-[218px] h-16 bg-[#D13F30] hover:bg-[#F96C5B] rounded-lg text-4xl font-bold shadow-[0_3px_0_0_#8F2316] text-white" on:click={() => calculate()}>=</button>
    </div>
  </div>

</main>

<style>

  .button {
    @apply w-24 h-16 bg-[#EAE3DB] rounded-lg text-4xl font-bold;
    box-shadow: 0px 3px 0px #B6A499;
  }

  .button:hover {
    @apply bg-[#FFFFFF];
  }

  #display {
    font-size: 48px;
  }

</style>