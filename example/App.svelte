<script>
  import Input from './Input.svelte';
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
</style>

<main>
  <h1>Svelte mask input</h1>
  <p>
    A Svelte component for creating formatted inputs. It allows you to create credit card input, phone numbers, money
    and other kinds of mask input.
  </p>
  <h3>Classic usage: credit card</h3>
  <p>
    <Input alwaysShowMask maskChar="_" mask="0000-0000-0000-0000" />
  </p>
  <p>
    <CreditCard />
  </p>
  <p>
    MaskInput with preseted value:
    <Input alwaysShowMask maskChar="_" mask="0000-|0|-0000" defaultValue="123456789" />
  </p>
  <p>
    MaskInput with manageble value:
    <ManagableValue />
  </p>
  <p>
    Date input with custom year (2 (9x) or 4 numbers):
    <DateInput />
  </p>
  <p>
    Date input with placeholder (using previous component):
    <DateInput placeholder="Enter your birthdate DD.MM.YYYY" alwaysShowMask={false} />
  </p>

</main>
