## Mask input with simple API and rich customization.

If you need to create an input for:

- credit card
- phone number
- date
- birthday
- numbers
- Or other custom mask

This project could help you in all this situations!

Take a look at our demos: https://codesandbox.io/s/svelte-input-mask-demo-xurgr

### How to use it:

Install it:

```
npm install --save svelte-input-mask
```

or if you're using yarn:

```
yarn add svelte-input-mask
```

Import `MaskInput` component:

```js
import MaskInput from "svelte-input-mask/MaskInput.svelte";
```

Use it (for example for CreditCard):

```js
<MaskInput alwaysShowMask maskChar="_" mask="0000-000000-00000" />
```

Add event listeners:

```js
<script>
  import MaskInput from 'svelte-input-mask/MaskInput.svelte';

  let mask = '0000-0000-0000-0000';

  const handleChange = ({ detail }) => {
    console.log(detail.inputState.maskedValue); // stores the value of input

    if (detail.inputState.maskedValue.indexOf('34') === 0 || detail.inputState.maskedValue.indexOf('37') === 0) {
      mask = '0000-000000-00000';
      return;
    }

    mask = '0000-0000-0000-0000';
  };
</script>

<MaskInput alwaysShowMask maskChar="_" {mask} on:change={handleChange} />
```

Congrats! You made the first masked input :)

Checkout more usecases here: https://codesandbox.io/s/romantic-franklin-xurgr

### Where to use?

Credit cards:

```js
<MaskInput alwaysShowMask maskChar="_" mask="0000-000000-00000" />
```

Phones (you still can change prefixes, country code like in credit card example):

```js
<MaskInput
  alwaysShowMask
  mask="+1 (000) 000 - 0000"
  size={20}
  showMask
  maskChar="_"
/>
```

Dates:

```js
<script>
  import MaskInput from 'svelte-input-mask/MaskInput.svelte';

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

<MaskInput alwaysShowMask {maskString} {mask} on:change={handleChange}/>
```

Numbers:

```js
<script>
  import NumberInput from 'svelte-input-mask/NumberInput.svelte';
</script>

<NumberInput />
```

### Which props it has?

Mask input has next props:

| Prop           | Default value | Description                                                                                                                                                                                            |
| -------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| value          | -             | The value of the input. Will be processed to masked one. In this case you can control the value of the component                                                                                       |
| defaultValue   | -             | The default value of the input. Will be applied only during the first render                                                                                                                           |
| maskString     | -             | The mask string to show if there are no filled chars. It's length should be the same as `mask`. Example: `'DD.MM.YYYY'`                                                                                |
| maskChar       | ''            | In case you don't need a custom string you can define only a definite char for mask. Example: `maskChar = '_'` and `mask = '0000-0000-0000-0000'` will give: `____-____-____-____`                     |
| mask           | -             | The mask of the input. Could be a credit card: `'0000-0000-0000-0000'`, date: `00.00.0000` or whatever you want :) Doesn't work if `reformat` prop is setted                                           |
| maskFormat     | regexp        | The regexp for custom formatting. You may use it if you want to define a specific mask. See example here: https://github.com/xnimorz/masked-input/blob/master/packages/input-core/src/index.ts#L16-L28 |
| alwaysShowMask | false         | Flag to show the mask                                                                                                                                                                                  |
| showMask       | false         | Show mask if there is any data in input                                                                                                                                                                |
| reformat       | -             | The function, which defines a custom formatting rules. In case if you can't describe the format only with mask (e.g. numbers). If you use this prop `mask` prop will be ignored                        |

Svelte mask input pass all props that it doesn't handle right to `input` html element.

### Quick start examples at local machine

```
git clone git@github.com:xnimorz/svelte-input-mask.git
cd svelte-input-mask/example
yarn install
yarn dev
```

### Requirements:

Svelte should be installed in your project. Check the minimal Svelte version here: https://github.com/xnimorz/svelte-input-mask/blob/master/package.json#L42
