<footer>
  <p>built with ♥️ by <a href="https://stordahl.dev">Jacob Stordahl</a></p>
  <p>&copy; {new Date().getFullYear()} | contribute on <a href="https://github.com">GitHub</a></p>
</footer>

<style>
  footer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 1.25rem;
    margin-top: 3rem;
    width: calc(100% - 2.5rem);
  }
  p {
    margin: 0;
  }
  @media screen and (min-width: 900px){
    footer {
      position: absolute;
      bottom: 0;
    }
  }
</style>