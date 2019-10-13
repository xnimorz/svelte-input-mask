<script>
  import MaskInput from 'svelte-input-mask/src/MaskInput.svelte';

  let value = '1234';
  const handleChange = e => {
    value = e.detail.inputState.maskedValue.replace('-', '');
  };
</script>

<p>Value is: {value}</p>
<MaskInput {...$$props} on:change={handleChange} mask={'0000-0000'} {value} />
