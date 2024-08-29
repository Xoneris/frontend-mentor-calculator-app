<script lang="ts">

  let display:Array<string|number> = []

  function addToDisplay(character:string|number) {
    display.push(character);
    display = [...display];
  }

</script>

<main class="w-screen h-screen flex justify-center items-center bg-gray-600">
  
  <div class="flex flex-col gap-4 w-[539px]">
    <header class="flex justify-between">
      <p>Calc</p>
      <div>Theme</div>
    </header>

    <div class="w-ful h-14 p-8 bg-gray-900 text-white text-right rounded-lg">
      {#each display as char}
        {char}
      {/each}
    </div>

    <div class="bg-gray-800 p-8 rounded-lg flex flex-wrap gap-6">
      <button class="button" on:click={() => addToDisplay(7)}>7</button>
      <button class="button" on:click={() => addToDisplay(8)}>8</button>
      <button class="button" on:click={() => addToDisplay(9)}>9</button>
      <button class="button" >DEL</button>
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
    </div>
  </div>

</main>

<style>

  .button {
    @apply w-24 h-16 bg-yellow-600 rounded-lg;
  }

</style>
