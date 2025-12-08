<script lang='ts'>
  import Fa from 'svelte-fa'
  import {faArrowLeft, faArrowRight} from '@fortawesome/free-solid-svg-icons'
</script>
<div class='carousel'>
  <div class='left'>
    <Fa icon={faArrowLeft} />
  </div>
  <div class='center'>
    <img src="https://placecats.com/400/400" />
  </div>
  <div class='right'>
    <Fa icon={faArrowRight} />
  </div>
</div>
<style lang='scss'>
  .carousel {
    display: flex;
    
  }
  .left, .right {
    display: flex;
    color: black;
    flex-grow: 0;
  }
  .center {
    flex-grow: 2;
  }
</style>

