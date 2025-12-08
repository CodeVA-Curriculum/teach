<script lang='ts'>
  import Fa from 'svelte-fa'
  import {faArrowLeft, faArrowRight} from '@fortawesome/free-solid-svg-icons'

  let { sols } = $props()

  let lessons = $state([])
  let loaded = $state(false)

  async function load() {
    let url = "https://curriculum.codevirginia.org/api/library/k-8-lessons/food-chain-algorithms.json"
    // for(const id of sols) {
    //   url += "sol=" + id + "&"
    // }
    const res = await (await fetch(url)).json()
    // https://docs.google.com/document/d/1_NNnWXrd5p8tU29M0L1M_lBuXGGJXRczbqOvGy-ZsOQ/edit
    const driveURL = res.frontmatter.links.drive.replace("https://docs.google.com/document/d/", '').replace('/edit', '')
    res.thumbnail = `https://curriculum.codevirginia.org/thumbnails/${driveURL}.png`
    lessons.push(res)
    console.log(lessons)
    loaded = true
  }
</script>
<div class='carousel'>
  <button disabled={!loaded} class='left'>
    <Fa icon={faArrowLeft} />
  </button>
  <article class='center'>
      {#if !loaded}
      <div class='empty'>
        <button onclick={load}>Load Resources</button>
        <p>Select standards, then click above to search!</p>
      </div>
      {:else}
      <!-- <div class='thumbnail'>
        <img src="https://curriculum.codevirginia.org/thumbnails/1_NNnWXrd5p8tU29M0L1M_lBuXGGJXRczbqOvGy-ZsOQ.png" />
      </div> -->
      <div class='info'>
        <span class='card-title'>{lessons[0].frontmatter.title}</span>
        <p>{@html lessons[0].content.replaceAll("<h2>Summary</h2>", "").replaceAll("<h2>Overview</h2>", "")}</p>
        <!-- <ul>
          <li><strong>Grade: </strong>XX</li>
          <li><strong>Subjects: </strong>XX</li>
          <li><strong>SOLs: </strong>XX</li>
        </ul> -->
        <a role="button" href="{lessons[0].frontmatter.links.drive}" target="_blank">View Details</a>
      </div>
    {/if}
  </article>
  <button disabled={!loaded} class='right'>
    <Fa icon={faArrowRight} />
  </button>
</div>
<style lang='scss'>
  @use "$lib/styles/theme.scss";
  .card-title {
    font-family: theme.$title-font;
    font-size: 14pt;
    font-weight: bold;
    padding-bottom: 10px;
  }
  .info {
    display: flex;
    flex-direction: column;
    font-size: 12pt;
    a {
      font-size: 12pt;
      padding:6px;
      margin: 0;
      width: 100%;
      flex-grow: 0;
    }
    p {
      margin-bottom: 12px;
      flex-grow: 1;
      overflow-y: hidden;
      max-height: 4rem;
      -webkit-mask-image: linear-gradient(to bottom, black 60%, transparent 100%);
      mask-image: linear-gradient(to bottom, black 60%, transparent 100%);
    }
    margin-left: 1rem;
    // background-color: pink;
  }
  .carousel {
    display: flex;
    // flex-direction: row;
    // background-color: pink;
  }
  .left, .right {
    display: flex;
    color: black;
    flex-grow: 0;
    margin: 0 0px;
    background-color: white;
    color: black;
    padding: 0 10px;
    align-items: center;
  }
  .center {
    display: flex;
    margin: 0 10px;
    flex-grow: 0;
    // background-color: powderblue;
    // min-width: 200px;
    img {
      height: 160px;
      min-width: 108px;
    }
  }
  ul {
    position: relative;
    right: 1rem;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  li {
    margin: 0;
    padding: 0;
  }
  .empty {
    text-align: center;
    p {
      margin-top: 4rem; 
      margin: 12px 0; 
    }
    button {
      background-color: white;
      color: black;
    }
  }
</style>

