<script>
  import Coffee from './coffee-cup.svg'
</script>

<header>
  <div>
    <h1>ctrl</h1>
    <p>a tool for managing</p>
    <p>executive dysfunction</p>
  </div>
  <div id="coffee">
    <a href="https://ko-fi.com/stordahl" target="_blank">
      <Coffee width={40}/>
    </a>
  </div>
</header>

<style>
  h1, p { margin: 0; }
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  h1 {
    font-size: 2.5rem;
  }

  header > *{
    margin:1.5rem;
  }

  #coffee {
      display: none;
    }

  @media screen and (min-width: 600px){
    #coffee {
      display: block;
    }
  }
</style>