<script>
  const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
  const operations = ["/", "x", "+", "-", "="];

  let selectedOperation = "";
  let display = "";
  let firstNumber = "";
  let secondNumber = "";
  let isDisplayingResults = false;

  function handleOperation(operation) {
    if (!firstNumber) return;
    if (operation === "=") {
      if (!secondNumber) return;
      const firstNum = parseFloat(firstNumber);
      const secondNum = parseFloat(secondNumber);

      let results = "";

      switch (selectedOperation) {
        case "/":
          results = (firstNum / secondNum).toFixed(2);
          break;
        case "x":
          results = (firstNum * secondNum).toFixed(2);
          break;
        case "+":
          results = (firstNum + secondNum).toFixed(2);
          break;
        case "-":
          results = (firstNum - secondNum).toFixed(2);
          break;
      }

      if (
        results[results.length - 1] === "0" &&
        results[results.length - 2] === "0"
      ) {
        results = results.slice(0, results.indexOf("."));
      }

      display = results;
      isDisplayingResults = true;
    }
    selectedOperation = operation;
  }

  function handleClear() {
    firstNumber = "";
    secondNumber = "";
    selectedOperation = "";
    display = "";
    isDisplayingResults = false;
  }

  function handleNumber(number) {
    if (isDisplayingResults) {
      handleClear();
    }
    if (display === "" && number === "0") return;
    if (number === "." && display.includes(".")) return;

    if (!selectedOperation) {
      if (display === "" && number === ".") {
        firstNumber = "0.";
        return (display = firstNumber);
      }
      if (firstNumber.length <= 6) {
        firstNumber = `${firstNumber}${number}`;
      }

      display = firstNumber;
    } else {
      if (display === firstNumber && number === ".") {
        secondNumber = "0.";
        return (display = secondNumber);
      }

      if (secondNumber.length <= 6) {
        secondNumber = `${secondNumber}${number}`;
      }
      display = secondNumber;
    }
  }
</script>

<main
  class="flex h-screen w-screen items-center justify-center bg-gradient-to-r from-gray-900 to-gray-600"
>
  <section id="calculator" class="w-72 rounded-md bg-gray-900 p-4 shadow-lg">
    <div id="results" class="flex h-14 w-auto justify-end text-3xl text-white">
      {display}
    </div>
    <div id="digits" class="flex gap-2">
      <div id="numbers" class="grid w-3/4 grid-cols-3 gap-2">
        <button
          class="col-span-3 h-14 rounded-full bg-gray-600 font-bold text-white hover:bg-gray-500 active:bg-gray-700"
          on:click={handleClear}>C</button
        >
        {#each numbers as number (number)}
          <button
            class="h-14 rounded-full bg-gray-600 font-bold text-white hover:bg-gray-500 active:bg-gray-700"
            class:col-span-3={number === "C"}
            class:col-span-2={number === "0"}
            on:click={() => handleNumber(number)}>{number}</button
          >
        {/each}
      </div>
      <div
        id="operations"
        class="col-span-1 flex flex-col items-center justify-between"
      >
        {#each operations as operation (operation)}
          <button
            class="h-14 w-14 rounded-full px-2 font-bold text-white {operation ===
            selectedOperation
              ? 'bg-gray-400'
              : 'bg-amber-500 hover:bg-amber-400 active:bg-amber-600'}"
            on:click={() => handleOperation(operation)}>{operation}</button
          >
        {/each}
      </div>
    </div>
  </section>
</main>
