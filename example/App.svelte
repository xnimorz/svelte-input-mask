<script>
  import MaskInput from 'svelte-input-mask/src/MaskInput.svelte';
  import NumberInput from 'svelte-input-mask/src/NumberInput.svelte';

  import CreditCard from './CreditCard.svelte';
  import ManagableValue from './ManagableValue.svelte';
  import DateInput from './DateInput.svelte';
</script>

<style>
  main {
    padding: 30px 20px;
    max-width: 600px;
    margin: auto;
  }
  /** 
  * We need to use global, as svelte currently doesn't 
  * support class on nested components: 
  * https://github.com/sveltejs/svelte/pull/2888 
  * https://github.com/sveltejs/svelte/issues/2870
  **/
  :global(.input) {
    display: inline-block;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    padding: 0 11px;
    margin: 0;
    border-radius: 3px;
    border: 1px solid #cbd1d4;
    font-size: 14px;
    font-family: Arial, Helvetica, sans-serif;
    color: #333;
    -webkit-appearance: none;
    -moz-appearance: none;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    -webkit-tap-highlight-color: transparent;
    background-color: #fff;
    height: 32px;
    padding: 0 10px;
    border: 1px solid #c6c9ca;
    border-radius: 2px;
  }

  :global(.input):focus {
    border-color: #0f8fee;
    outline: none;
    box-shadow: inset 0 2px 1px rgba(0, 0, 0, 0.1235);
  }

  :global(.input):hover {
    border-color: #acb5ba;
  }

  .example {
    padding: 0 0 13px;
  }

  p {
    margin-bottom: 5px;
  }
</style>

<main>
  <h1>Svelte mask input</h1>
  <p>
    <a target="_blank" href="https://github.com/xnimorz/svelte-input-mask">Svelte mask input on GitHub</a>
  </p>
  If you need to create an input for:
  <ul>
    <li>credit card</li>
    <li>phone number</li>
    <li>date</li>
    <li>birthday</li>
    <li>numbers</li>
    <li>Or other custom mask</li>
  </ul>
  This project could help you in all this situations!
  <h3>Classic usage: credit card</h3>
  <div class="example">
    <p>The most simple usage is a credit card:</p>
    <MaskInput class="input" size={20} alwaysShowMask maskChar="_" mask="0000-0000-0000-0000" />
  </div>
  <div class="example">
    <p>
      All props you can change right during the runtime. Credit card with automatic switching between visa and american
      express format (for amex format write 34 or 37):
    </p>
    <CreditCard class="input" />
  </div>
  <div class="example">
    <p>Date input with custom year (2 (9x) or 4 numbers):</p>
    <DateInput class="input" />
  </div>
  <div class="example">
    <p>You can use any regular input attrs, like placeholder:</p>
    <DateInput size={35} class="input" placeholder="Enter your birthdate DD.MM.YYYY" alwaysShowMask={false} />
  </div>
  <div class="example">
    <p>The USA phone format:</p>
    <MaskInput alwaysShowMask mask="+1 (000) 000 - 0000" size={20} showMask maskChar="_" />
  </div>
  <div class="example">
    <p>MaskInput with preseted value:</p>
    <MaskInput class="input" alwaysShowMask maskChar="_" mask="0000-|0|-0000" defaultValue="123456789" />
  </div>
  <div class="example">
    <p>MaskInput with manageble value:</p>
    <ManagableValue class="input" />
    <p />
  </div>
  <div class="example">
    <p>Number formatter:</p>
    <NumberInput class="input" />
  </div>

</main>
