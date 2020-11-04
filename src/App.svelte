<script>
  import {onMount} from 'svelte';
  import { Octokit } from "@octokit/rest";
  import Timeline from './Timeline.svelte';

  let count = 0;
  let username;
  let repos = { data: [] };
  onMount(() => {
    const interval = setInterval(() => count++, 1000);
    return () => {
      clearInterval(interval);
    };
  });

  async function generateTimeLine() {
    const octokit = new Octokit();
    repos = await octokit.repos.listForUser({
      username,
      type: 'owner',
      sort: 'created',
      direction: 'desc'
    });
  }
</script>

<style>
  :global(body) {	
  margin: 0;
  padding: 0;
  background: rgb(230,230,230);
  
  color: rgb(50,50,50);
    font-family: 'Open Sans', sans-serif;
    font-size: 112.5%;
    line-height: 1.6em;
  }

  .App {
    text-align: center;
    padding: 1em;
  }

  .input-field {
    padding: 1em;
    font-size: .8em;
    border-radius: 0.25em;
  }

  .button {
    padding: 1em;
    border-radius: 0.25em;
    background: rgb(255, 80,80);
    color: #fff;
    border: none;
    cursor: pointer;
    font-size: .8em;
  }

  .button:hover {
    background: rgb(255, 40,40);
  }

  footer {
    margin-top: 2em;
  }

</style>

<div class="App">
  <h1>Github Timeline</h1>
  <form>
    <label for="txtUsername">Github user name:</label>
    <input id="txtUsername" type="text" bind:value={username} class="input-field"/>
    <button class="button" type="button" on:click={generateTimeLine}>Generate Timeline</button>
  </form>
  {#if repos.data.length > 0 }
    <Timeline repos={repos.data}/>
  {:else}
    <p>Loading, please wait...</p>
  {/if}
  <footer>
    <a href="https://github.com/rajasegar/github-timeline-svelte">Github</a>
  </footer>
</div>
