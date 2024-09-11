<script>

  export let data
 
  function checkAvatarImage (avatar) {

    if (avatar.includes('https')) {

      return true

    }
 
    return false

  }
</script>
 
<main>
 
  <ul>

    {#each data.persons as person}
<li>
<span class="avatar">

            {#if person.avatar.length > 0 && checkAvatarImage (person.avatar)}
<img src="{person.avatar}" alt="Avatar van {person.name}">

            {:else if person.avatar.length > 0 || checkAvatarImage (person.avatar)}

              {person.avatar}

            {:else}
<img src="https://robohash.org/mail@ashallendesign.co.uk" alt="Avatar van {person.name}">

            {/if}
</span>
 
          {person.name}
</li>

    {/each}
</ul>
</main>
 
<style>
</style>
 