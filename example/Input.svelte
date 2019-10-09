<script>
  import { createEventDispatcher, tick } from 'svelte';
  import { createInput, defaults } from 'input-core';

  export let value = undefined;
  export let defaultValue = undefined;
  export let reformat = undefined;
  export let maskString = undefined;
  export let maskChar = defaults.maskChar;
  export let mask = defaults.mask;
  export let maskFormat = defaults.maskFormat;
  export let alwaysShowMask = false;
  export let showMask = false;

  const KEYBOARD = {
    BACKSPACE: 8,
    DELETE: 46,
  };
  const dispatch = createEventDispatcher();

  const input = createInput({
    value: value || defaultValue || '',
    reformat,
    maskString,
    maskChar,
    mask,
    maskFormat,
  });

  let shouldShowMask = alwaysShowMask || showMask;
  $: {
    shouldShowMask = alwaysShowMask || showMask;
    showValue();
  }
  $: {
    input.setReformat(reformat);
    showValue();
  }
  $: {
    input.setMaskFormat(maskFormat);
    showValue();
  }
  $: {
    input.setMask(mask);
    showValue();
  }
  $: {
    input.setMaskString(maskString);
    showValue();
  }
  $: {
    input.setMaskChar(maskChar);
    showValue();
  }
  $: {
    value !== undefined && input.setValue(value);
    showValue();
  }

  const {
    value: _value,
    defaultValue: _defaultValue,
    reformat: _reformat,
    maskString: _maskString,
    maskChar: _maskChar,
    mask: _mask,
    maskFormat: _maskFormat,
    alwaysShowMask: _alwaysShowMask,
    showMask: _showMask,
    ...other
  } = $$props;

  let inputValue = value || defaultValue || '';
  let inputEl;
  let canSetSelection = false;

  function applyValue(value) {
    input.setValue(value);
    showValue();
  }

  function showValue() {
    if (shouldShowMask && (canSetSelection || alwaysShowMask)) {
      inputValue = input.getMaskedValue();
      return;
    }
    inputValue = input.getVisibleValue();
  }

  async function setSelection() {
    if (!canSetSelection) {
      return;
    }
    const selection = input.getSelection();

    await tick();
    inputEl.setSelectionRange(selection.start, selection.end);
    const raf =
      window.requestAnimationFrame ||
      window.webkitRequestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      (fn => setTimeout(fn, 0));
    // For android
    raf(() => inputEl.setSelectionRange(selection.start, selection.end));
  }

  function getSelection() {
    input.setSelection({
      start: inputEl.selectionStart,
      end: inputEl.selectionEnd,
    });
  }

  function handleInput(e) {
    let currentValue;
    if (showMask && (canSetSelection || alwaysShowMask)) {
      currentValue = input.getMaskedValue();
    } else {
      currentValue = input.getVisibleValue();
    }

    // fix conflict by update value in mask model
    if (e.target.value !== currentValue) {
      getSelection();
      input.setValue(e.target.value);

      showValue();

      setTimeout(setSelection, 0);
    }

    dispatchChangeEvent(e);
  }

  function handlePaste(e) {
    e.preventDefault();
    getSelection();

    // getData value needed for IE also works in FF & Chrome
    input.paste(e.clipboardData.getData('Text'));

    showValue();

    // Timeout needed for IE
    setTimeout(setSelection, 0);

    dispatchChangeEvent(e);
  }

  function handleKeyPress(e) {
    if (e.metaKey || e.altKey || e.ctrlKey || e.key === 'Enter') {
      return;
    }

    e.preventDefault();
    getSelection();
    input.input(e.key || e.data || String.fromCharCode(e.which));
    showValue();
    setSelection();

    dispatchChangeEvent(e);
  }

  function handleKeyDown(e) {
    if (e.which === KEYBOARD.BACKSPACE) {
      e.preventDefault();
      getSelection();
      input.removePreviosOrSelected();

      showValue();
      setSelection();

      dispatchChangeEvent(e);
    }

    if (e.which === KEYBOARD.DELETE) {
      e.preventDefault();
      getSelection();
      input.removeNextOrSelected();

      showValue();
      setSelection();

      dispatchChangeEvent(e);
    }
  }

  function handleFocus(e) {
    canSetSelection = true;
    dispatch('focus', e);
  }

  function handleBlur(e) {
    canSetSelection = false;
    dispatch('blur', e);
  }

  function dispatchChangeEvent(e) {
    const { maskedValue, visibleValue } = input.getState();
    dispatch('change', { originalEvent: e, inputState: { maskedValue, visibleValue } });
  }

  //   function keyPressPropName() {
  //     if (
  //       typeof navigator !== "undefined" &&
  //       navigator.userAgent.match(/Android/i)
  //     ) {
  //       return "beforeinput";
  //     }
  //     return "keypress";
  //   }

  showValue();

  console.log(other);
</script>

<input
  {...other}
  value={inputValue}
  on:input={handleInput}
  on:keydown={handleKeyDown}
  on:keypress={handleKeyPress}
  on:paste={handlePaste}
  on:focus={handleFocus}
  on:blur={handleBlur}
  bind:this={inputEl} />
