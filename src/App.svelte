<script>
  import AutoComplete from "./SimpleAutocomplete.svelte";
  import { countryList } from "./country-list.js";

  const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];
  let selectedColor;
  let items = countryList;
  let selectedCountry;

  let disabled = false;
  let showPlacehoder = false;

  const example1Given = `const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];`;

  const example1Code = `const colors = ["White", "Red", "Yellow", "Green", "Blue", "Black"];
let selectedColor;
<AutoComplete items={colors} bind:selectedItem={selectedColor} />
Selected color: {selectedColor}`;

  // --------------------

  const colorList = [
    { id: 1, name: "White", code: "#FFFFFF" },
    { id: 2, name: "Red", code: "#FF0000" },
    { id: 3, name: "Yellow", code: "#FF00FF" },
    { id: 4, name: "Green", code: "#00FF00" },
    { id: 5, name: "Blue", code: "#0000FF" },
    { id: 6, name: "Black", code: "#000000" }
  ];

  let selectedColorObject;
  let selectedColorValue;

  const example2Code = `const colorList = [
  { id: 1, name: "White", code: "#FFFFFF" },
  { id: 2, name: "Red", code: "#FF0000" },
  { id: 3, name: "Yellow", code: "#FF00FF" },
  { id: 4, name: "Green", code: "#00FF00" },
  { id: 5, name: "Blue", code: "#0000FF" },
  { id: 6, name: "Black", code: "#000000" }
];

let selectedColorObject;
let selectedColorValue;

<AutoComplete
	items={colorList}
	bind:selectedItem={selectedColorObject}
	bind:value={selectedColorValue}
	labelFieldName="name"
	valueFieldName="id"
  keywordsFunction={color => color.name + ' ' + color.code} />`;

  async function searchCountry(keyword) {
    const url =
      "https://restcountries.eu/rest/v2/name/" +
      encodeURIComponent(keyword) +
      "?fields=name;alpha2Code";

    const response = await fetch(url);
    const json = await response.json();

    return json;
  }
</script>

<style>

</style>

<section class="section">
  <div class="container content">
    <h1>Svelte Simple Autocomplete Demo</h1>

    <p>
      Github:
      <a href="https://github.com/pstanoev/simple-svelte-autocomplete">
        https://github.com/pstanoev/simple-svelte-autocomplete
      </a>

    </p>
    <h3>Simple example:</h3>

    <div class="columns">
      <div class="column is-one-third">
        <h5>Pick a color:</h5>

        <AutoComplete items={colors} bind:selectedItem={selectedColor} />
        <p>Selected color: {selectedColor}</p>

      </div>
      <div class="column">
        <pre>
          <code class="language-html" data-lang="html">{example1Code}</code>
        </pre>
      </div>
    </div>

    <h3>Advanced example:</h3>
    <p>
      If you have an array of objects as items, they can be feed directly into
      the AutoComplete component and you can specify which field to use as
      label.
      <br />
      You can also specifiy function to combine multiple fields to search by.
      Try searching by HEX code of a color.
    </p>

    <div class="columns">
      <div class="column is-one-third">
        <h5>Pick a color:</h5>
        <AutoComplete
          items={colorList}
          bind:selectedItem={selectedColorObject}
          bind:value={selectedColorValue}
          labelFieldName="name"
          valueFieldName="id"
          keywordsFunction={color => color.name + ' ' + color.code}
          {disabled}
          placeholder={showPlacehoder ? 'Please select color' : ''} />

        <p>
          Selected color item: {JSON.stringify(selectedColorObject)}
          <br />
          Selected value: {selectedColorValue}
        </p>

        <div>
          <p>Change selected item from outside:</p>
          <button
            class="button"
            on:click={() => (selectedColorObject = colorList[colorList.length - 1])}>
            Set color to Black
          </button>
        </div>
      </div>
      <div class="columns">
        <div class="column">
          <pre>
            <code class="language-html" data-lang="html">{example2Code}</code>
          </pre>
        </div>
      </div>
    </div>

    <h3>Async example:</h3>
    <h5>Pick a country:</h5>
    <AutoComplete
      searchFunction={searchCountry}
      bind:selectedItem={selectedCountry}
      labelFieldName="name"
      maxItemsToShowInList="10" />

    <div style="margin-bottom: 10rem;">
      <p>Selected country: {JSON.stringify(selectedCountry)}</p>
    </div>

  </div>
</section>
