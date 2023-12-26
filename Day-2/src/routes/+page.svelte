<script>
	import Counter from '$lib/Counter.svelte';

 let data = { name: 'Svelte Bot' };
  let cookieCount = 0;
</script>


<Counter bind:count={cookieCount} />

<button on:click={() => (cookieCount += 1)}>Cookie Ekle</button>
<button on:click={() => (cookieCount -= 1)}>Cookie Sil</button>
<button on:click={() => (cookieCount = 0)}>Reset Sayaç</button>


