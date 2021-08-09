<script>
  import { slide } from 'svelte/transition'
  import Fireicon from '$lib/Fireicon.svelte'
  let topNews = []
  let total = 0
  let got = 0
  let getTopNews = async () => {
    total = 0
    topNews = []
    got = 0
    let response = await fetch('https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty')
    let newsIds = await response.json()
    total = newsIds.length
    let promiseList = []
    for (let id of newsIds) {
      let f = async () => {
        let news = await getNewsDetails(id)
        topNews = [...topNews, news]
        got++
      }
      promiseList.push(f())
    }
    await Promise.all(promiseList)
  }
  let getNewsDetails = async (id) => {
    let response = await fetch(`https://hacker-news.firebaseio.com/v0/item/${id}.json?print=pretty`)
    let newsDetails = await response.json()
    return newsDetails
  }
</script>

<h1>
  News {#if total > 0 && got == total}({total}){/if}
</h1>

{#if total > 0 && got < total}
  <div class="row">
    <progress value={got / total} />
    ({got}/{total})
  </div>
{/if}
<button class="btn btn-primary" on:click={getTopNews}>Get Top News</button>
<ul>
  {#each topNews as news}
    <li transition:slide|local>
      <a href={news.url}>{news.title}</a>

      {#if news.score > 100}
        <Fireicon />
      {/if}
      ({news.score})
    </li>
  {/each}
</ul>
