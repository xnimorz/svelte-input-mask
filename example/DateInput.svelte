<script>
  import MaskInput from 'svelte-input-mask';

  let maskString = 'DD.MM.YYYY';
  let mask = '00.00.0000';

  const handleChange = ({ detail }) => {
    const value = detail.inputState.maskedValue;
    if (parseInt(value[6], 10) > 2) {
      maskString = 'DD.MM.YY';
      mask = '00.00.00';
    } else {
      maskString = 'DD.MM.YYYY';
      mask = '00.00.0000';
    }
  };
</script>

<MaskInput alwaysShowMask {maskString} {mask} on:change={handleChange} {...$$props} />
