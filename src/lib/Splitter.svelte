<script>
  import Dollar from '../assets/icon-dollar.svg';
  import Person from '../assets/icon-person.svg';

  const tipSizes = [5, 10, 15, 25, 50];

  let bill = undefined; 
  let people = undefined;
  let customPercentage = undefined;
  let percentage = 0;
  
  $: totalTip = calculateTip(bill, people, percentage, customPercentage);
  $: total = calculateTotal(totalTip, people);

  function calculateTip(bill, people, percentage, customPercentage) {
    let tip = 0;

    if (bill && people) {
      if (customPercentage) {
        percentage = customPercentage;
      } 

      // Calculate tip
      tip = bill * percentage / 100 / people;
    }

    return tip;
  }

  function calculateTotal(totalTip, people = 1) {
    let total = 0;

    // Calculate totals
    total = totalTip * people;

    return total;
  }

  function updatePercentage(evt) {
    const selectedPercentage = evt.target.dataset.percentage;

    customPercentage = undefined;
    percentage = selectedPercentage;
  }

  function currency(amount) {
    return '$' + Number.parseFloat(amount).toFixed(2);
  }

  function reset() {
    bill = undefined; 
    people = undefined;
    customPercentage = undefined;
    clearPercentage();
  }

  function clearPercentage() {
    percentage = 0;
  }

  function validate(evt) {
    const element = evt.target;
    const parent = element.parentNode;
    const value = parseFloat(element.value);
    let error = '';
    let min = parseFloat(element.attributes.min.value);
    let max = parseFloat(element.attributes.max.value);
    let step = parseFloat(element.attributes.step.value);

    if (value !== undefined && value < min + step) {
      error = "Can't be zero";
    }

    if (value !== undefined && value > max) {
      error = "Can't exceed " + max;
    }
    
    if (error) {
      parent.classList.add('error');
      parent.dataset.error = error;
    } else {
      parent.classList.remove('error');
      parent.dataset.error = '';
    }
  }

</script>

<section class="calculator">
  <fieldset class="user-input">
    <legend>Bill</legend>
    <div class="input-field">
      <label for="bill">Bill</label>
      <span class="input-icon"><img src={ Dollar } alt=""/></span>
      <input id="bill" class="first second" type="number" on:blur={validate} step="0.01" min="0" max="9999" bind:value={bill} placeholder="0">
      <span class="error-message"></span> 
    </div>
  </fieldset>

  <fieldset class="user-select">
    <legend>Select Tip %</legend>
    <div class="select-group">
      {#each tipSizes as size}
        <button class:active={ percentage == size } on:click={updatePercentage} data-percentage={size}>{size}%</button>
      {/each}
      <div class="input-field">
        <label for="amount">Custom Tip %</label>
        <input id="amount" type="number" on:blur={validate} step="1" min="0" max="100" bind:value={customPercentage} on:focus={clearPercentage} placeholder="Custom">
      </div>
    </div>
  </fieldset>

  <fieldset class="user-input">
    <legend>Number of People</legend>
    <div class="input-field">
      <label for="people">Number of People</label>
      <span class="input-icon"><img src={ Person } alt=""/></span>
      <input id="people" type="number" on:blur={validate} step="1" min="0" max="10" bind:value={people} placeholder="0"> 
    </div>
  </fieldset>
</section>

<section class="card">
    <div class="totals">
        <div class="total">
            <div class="total-label">Tip Amount <span>/ person</span></div>
            <div class="total-amount">{currency(totalTip)}</div>
        </div>
        <div class="total">
            <div class="total-label">Total <span>/ person</span></div>
            <div class="total-amount">{currency(total)}</div>
        </div>
    </div>
    <button class="reset" on:click={reset} disabled={total ? false : true} tabindex="0">Reset</button>
</section>
