<script lang="ts">
  export let tool;
</script>

<div id="container">
  <div>
    <a target="_blank" href={tool.link}>
      {tool.title}
    </a>
    <button on:click> remove </button>
  </div>
  <p>{tool.description}</p>

  {#each tool.tags as tag}
    <span><b>#{tag} </b></span>
  {/each}
</div>

<style>
  #container {
    background: #ffffff 0% 0% no-repeat padding-box;
    box-shadow: 0px 5px 7px #0000000d;
    border: 1px solid #ebeaed;
    border-radius: 5px;

    margin: 32px 0;
    padding: 16px;
  }

  #container div {
    display: flex;
    justify-content: space-between;
  }

  #container div a {
    text-align: left;
    font: normal normal 600 30px/36px "Source Sans Pro";
    color: #365df0;
    letter-spacing: 0.6px;
  }

  #container div button {
    text-align: center;
    font: normal normal 600 18px/24px "Source Sans Pro";
    letter-spacing: 0.36px;
    color: #f95e5a;
    background-color: inherit;
    border: none;

    cursor: pointer;
  }

  #container div button:disabled {
    color: #fcaeac;
    cursor: not-allowed;
  }

  #container span {
    margin-right: 8px;
    font-weight: 600;
  }
</style>
