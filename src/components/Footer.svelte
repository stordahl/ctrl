<script>
  import themes from './themes'
  import ThemeSelector from './ThemeSelector.svelte'
  export let ifTasksIsLong;
  let styles;

  $: ifTasksIsLong ? styles = 'footer { position: absolute; bottom: 0;}': styles = ' ';
</script>

<footer style={ styles }>
  <ThemeSelector {themes}/>
  
  <div style="display:flex; flex-direction: column; align-items: flex-end;">
    <p>built with ♥️ by <a href="https://stordahl.dev">Jacob Stordahl</a></p>
    <p>&copy; {new Date().getFullYear()} | contribute on <a href="https://github.com/stordahl/ctrl">GitHub</a></p>
  </div>
</footer>

<style>
  footer {
    position: relative;
    z-index: 0;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    margin: 1.25rem;
    margin-top: 3rem;
    width: calc(100% - 2.5rem);
    background: var(--bg);
  }
  p {
    margin: 0;
  }
  @media screen and (min-width:500px){
    footer {
      flex-direction: row;
    }
  }
  @media screen and (min-width: 900px){
    footer {
      position: fixed;
      bottom: 0;
    }
  }
</style>