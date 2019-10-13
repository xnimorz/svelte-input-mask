<script>
  import MaskInput from 'svelte-input-mask';

  let mask = '0000-0000-0000-0000';

  const handleChange = ({ detail }) => {
    if (detail.inputState.maskedValue.indexOf('34') === 0 || detail.inputState.maskedValue.indexOf('37') === 0) {
      mask = '0000-000000-00000';
      console.log('changed');
      return;
    }

    mask = '0000-0000-0000-0000';
  };
</script>

<MaskInput {...$$props} alwaysShowMask maskChar="_" {mask} on:change={handleChange} />
