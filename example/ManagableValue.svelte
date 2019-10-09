<script>
  import Input from './Input.svelte';

  let value = '1234';
  const handleChange = e => {
    value = e.detail.inputState.maskedValue.replace('-', '');
  };
</script>

<p>Value is: {value}</p>
<Input on:change={handleChange} mask={'0000-0000'} {value} />
