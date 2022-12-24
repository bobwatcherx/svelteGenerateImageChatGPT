<script>
import {Configuration,OpenAIApi} from 'openai'
const config = new Configuration({
  apiKey:import.meta.env.VITE_API_KEY
})

const openai = new OpenAIApi(config)
let resultdata  = []
let youmessage = ""
let yousize = "medium"

const mygenerate = async()=>{
  try{
    const res = await openai.createImage({
      prompt:youmessage,
      n:1,
      size:yousize
    })
    const imageurl = res.data.data[0].url
    resultdata = [...resultdata,{prompt:youmessage,data:imageurl}]
  }
  catch(error){
    console.log(error)
  }

}
</script>

{#if resultdata.length > 0 }
  <div class="result_image">
    {#each resultdata as r}
    <div class="card_image">
        <img src={r.data} style="width: 140px;" alt="">
        <h3>{r.prompt}</h3>
    </div>
    {/each}
  </div>
{/if}

<!-- INPUT TEXT -->
<input type="text" bind:value={youmessage} class="inputtext">

<!-- SELECT INPUT FOR SIZE -->
<select bind:value={yousize} id="">
  <option value="256x256">small</option>
  <option value="512x512">medium</option>
  <option value="1024x1024">large</option>
</select>

<button on:click={mygenerate}
  class="btnsend"
>Send</button>

<style>
  .result_image{
    box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.5);
    border-radius: 30px;
    box-sizing: border-box;
    padding: 30px;
  }
  .card_image{
    display: block;
  }

  .inputtext{
    height: 40px;
    font-size: 30px;
    font-weight: bold;
    padding: 10px;
  }
  .btnsend{
    background-color: cyan;
    color: black;
    height: 70px;
  }
</style>