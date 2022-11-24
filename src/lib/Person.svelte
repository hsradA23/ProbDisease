<script>
  // export let diseased;
  // let getColor = (d) => {if(d==0) return color = "green"; else if(d > 0 && d < 3){return color = "red"} else return color = "gray" };
  // $: color = getColor(diseased);
  export let color;
</script>

<div class="personBox" style="background-color: {color};">

</div>


<style>
  .personBox{
    /* border-style: solid; */
    width: 0.25em;
    height: 0.25em;
    margin: 0.125em;
    border-radius: 0.125em;
    scale: 1.5;
  }
</style>