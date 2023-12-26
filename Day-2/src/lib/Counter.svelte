<script>
  import { onMount, beforeUpdate } from 'svelte';
  import { fly, fade } from 'svelte/transition';

  export let count = 0;
  export let mood = '';

  let prevCount = 0;

  onMount(() => {
    prevCount = count;
  });

  beforeUpdate(() => {
    if (count > prevCount) {
      mood = '🎅 Mutlu';
    } else if (count < prevCount) {
      mood = '😠 Kızgın';
    } else {
      mood = '😐 Nötr';
    }

    prevCount = count;
  });
</script>

{#key count}
  <div transition:fly="{{ y: 200, duration: 200 }}">
    <p>{count} cookie yendi!</p>
  </div>
{/key}

<p transition:fade="{{ duration: 500 }}">
  Santa's mood: {mood}
</p>
