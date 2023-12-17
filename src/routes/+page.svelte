<script>
  let children = [];

  async function fetchData() {
    try {
      const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-one.json');
      const data = await response.json();
      children = data;
    } catch (error) {
      console.error('Veri çekme hatası:', error);
    }
  }

  function evaluateChild(tally) {
    return tally >= 0 ? 'hoş' : 'yaramaz';
  }
</script>

<main>
  <h1>Çocuk Değerlendirme</h1>

  <button on:click={fetchData}>Verileri Çek</button>

  {#if children.length > 0}
    <ul>
      {#each children as child (child.name + child.tally)}
    <li key={child.name + child.tally}>
        {child.name} - {child.tally} - {evaluateChild(child.tally)}
    </li>
		{/each}
    </ul>
  {:else}
    <p>Verileri çekmek için lütfen "Verileri Çek" butonuna tıklayın.</p>
  {/if}
</main>
