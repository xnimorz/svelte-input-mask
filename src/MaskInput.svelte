<script>
  import { createEventDispatcher, tick, onMount, onDestroy } from 'svelte';
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
  $: shouldShowMask = alwaysShowMask || showMask;
  $: input.setReformat(reformat);
  $: input.setMaskFormat(maskFormat);
  $: input.setMask(mask);
  $: input.setMaskString(maskString);
  $: input.setMaskChar(maskChar);
  $: value !== undefined && input.setValue(value);

  onMount(() => {
    input.subscribe(applyValue);
  });

  onDestroy(() => {
    input.unsubscribe(applyValue);
  });

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

  let canSetSelection = false;
  let inputValue = setupInputValue(input.getState());

  let inputEl;

  function setupInputValue({ maskedValue, visibleValue }) {
    if (shouldShowMask && (canSetSelection || alwaysShowMask)) {
      return maskedValue;
    }
    return visibleValue;
  }

  function applyValue({ maskedValue, visibleValue, selection }) {
    inputValue = setupInputValue({ maskedValue, visibleValue });
    setSelection(selection);
    dispatchChangeEvent({ maskedValue, visibleValue });
  }

  async function setSelection({ start, end }) {
    if (!canSetSelection) {
      return;
    }

    await tick();
    inputEl.setSelectionRange(start, end);
    const raf =
      window.requestAnimationFrame ||
      window.webkitRequestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      (fn => setTimeout(fn, 0));
    // For android
    raf(() => inputEl.setSelectionRange(start, end));
  }

  function setupSelection() {
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
      setupSelection();
      input.setValue(e.target.value);
      setSelection(input.getSelection());
      setTimeout(() => setSelection(input.getSelection()), 0);
    }    
  }

  function handlePaste(e) {
    e.preventDefault();
    setupSelection();

    // getData value needed for IE also works in FF & Chrome
    input.paste(e.clipboardData.getData('Text'));
    setSelection(input.getSelection());
    // Timeout needed for IE
    setTimeout(() => setSelection(input.getSelection()), 0);    
  }

  function handleKeyPress(e) {
    if (e.metaKey || e.altKey || e.ctrlKey || e.key === 'Enter') {
      return;
    }

    e.preventDefault();
    setupSelection();
    input.input(e.key || e.data || String.fromCharCode(e.which));
    setSelection(input.getSelection());  
  }

  function handleKeyDown(e) {
    if (e.which === KEYBOARD.BACKSPACE) {
      e.preventDefault();
      setupSelection();
      input.removePreviosOrSelected();
      setSelection(input.getSelection());      
    }

    if (e.which === KEYBOARD.DELETE) {
      e.preventDefault();
      setupSelection();
      input.removeNextOrSelected();
      setSelection(input.getSelection());
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

  function dispatchChangeEvent({ maskedValue, visibleValue }) {
    dispatch('change', { element: inputEl, inputState: { maskedValue, visibleValue } });
  }
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
