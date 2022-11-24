<script>
    export let size;
    export let days;
    export let spreadProbInp; 
    $: spreadProb = spreadProbInp;
    export let sickDurationInp; $: sickDuration = sickDurationInp;
    export let delayInp; $: delay = delayInp;
    import Person from './Person.svelte'
    // let people = Array(size*size).fill().map(() =>Math.random() > 0.998);
    let getColor = (d) => {if(d==0) return "green"; else if(d > 0 && d <= sickDuration){return "red"} else return "gray" };
    let csv = "sick,infected,recovered\n";
    
    
    let people = Array(size*size).fill().map(() =>Number(Math.random() > 0.999));
    $: colorArr = people.map(getColor);
    
    export const reset = () => {
        location.reload();1
    }
    //Function that runs each day
    let onTick = (iteration) => {
        let changed = []
        for(let i = 0; i < size*size;i++ ){
            if(changed.includes(i)){
                continue;
            }
            if(people[i]>0 && people[i]<= sickDuration){
                // If person is diseased
                let neighbours = []
                if(!(i%size == 0)){
                    //left limit
                    neighbours.push(i-1);
                }
                if(!(i>=0 && i< size)){
                    //top limit
                    neighbours.push(i-size);
                }
                if( (i+1)%size !=0 ){
                    //right limit
                    neighbours.push(i+1);
                }
                if( i < ((size-1)*size) ){
                    //bottom limit
                    neighbours.push(Number(i)+Number(size));
                }
                changed = changed.concat(neighbours);
                // console.log(neighbours);
                for(let j = 0; j < neighbours.length; j++){
                    if(!people[neighbours[j]])
                    people[neighbours[j]] = Number(Math.random()>spreadProb);
                }

                people[i]++;
            }
        }

        // Get S  I and R numbers
        let s = 0; let i = 0;let r = 0;
        for(let idx = 0 ; idx < people.length ; idx++){
            if(people[idx] == 0) s++;
            else if(people[idx] >0 && people[idx] <= sickDuration) i++;
            else r++;
        }
        csv += s + ',' + i + ',' + r + '\n';
        if(iteration == (days-1)) console.log(csv);

    };

    function run(){
        spreadProb = 1-spreadProbInp;
        sickDuration = sickDurationInp;
        delay = delayInp;
        for (let i = 0; i < days; i++) {
            setTimeout(function timer() {
            onTick(i);
        }, i * delay);
    }
}
</script>
  
<!-- svelte-ignore a11y-click-events-have-key-events -->

<div class="communityBox" on:click={run} style="width:{size/2}em;height:{size/2}em">
    {#each people as person,idx}
        <Person bind:color={colorArr[idx]}/>
    {/each}
    
</div>
  
<style>
    .communityBox{
        background-color: #97c1cc;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-content: space-around;
        justify-content: space-evenly;
        border-radius: 0.25em;

    }
    
</style>
  