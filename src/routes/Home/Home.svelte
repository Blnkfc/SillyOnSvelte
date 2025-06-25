<script lang="ts">
  import { API_STRING } from "../../../constants";
  import axios from "axios";

  let currentPage = $state(1);
  let data = $state<any>([]);

  $effect(() => {
    const result = axios.get(API_STRING + `artworks?page=${currentPage}&limit=10&fields=title`).then((res) => {
        data = res.data.data
        console.log('data', data);
        
    });
  });
</script>

<div>
    {#each data as item}
        <div>{item.title}</div>
    {/each}

</div>
