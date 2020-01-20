<script>
  import MaskInput from 'svelte-input-mask';

  let unmasked = '1234';
  let masked = '1234-';
  let visible = '1234-';
  const handleChange = e => {
    unmasked = e.detail.inputState.unmaskedValue;
    masked = e.detail.inputState.maskedValue;
    visible = e.detail.inputState.visibleValue;
  };
</script>

<p>Unmasked value is: {unmasked}</p>
<p>Masked value is: {masked}</p>
<p>Visible value is: {visible}</p>
<MaskInput {...$$props} on:change={handleChange} mask={'0000-0000'} value={unmasked} />
