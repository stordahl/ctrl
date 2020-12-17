<script>
  import { onMount } from 'svelte'

  export let themes

  let prevTheme
  let currentTheme = 'minty' 
  let rootElem

  $: changeTheme = async (elem) => {
    if(elem.classList){
      await elem.classList.remove(prevTheme)
    }
    await elem.classList.add(currentTheme)
    prevTheme = currentTheme
  }

  onMount(() => {
    rootElem = document.body
  });  
</script>

<div>
  <label for="themes">themes</label>
  <!-- svelte-ignore a11y-no-onchange -->
  <select bind:value={currentTheme} name="themes" id="themes" on:change={() => changeTheme(rootElem)}>
    {#each themes as theme}
    <option value={theme.name}>{theme.name}</option>
    {/each}
  </select>
</div>

<style>
  label {
    font-size: 0.7rem
  }

  select {
    width: 100px;
    margin-bottom: 2rem;
  }

  @media screen and (min-width:500px) {
    select {
      margin-bottom: 0;
    }
  }
</style>