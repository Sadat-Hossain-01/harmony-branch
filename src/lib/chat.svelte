<script lang="ts">
  import { linear } from "svelte/easing";
  import { scale } from "svelte/transition";
  import Avatar from "./avatar.svelte";
  export let poster = "";
  export let time = ""; //when it was posted
  export let dp: string | null = null;
  export let post: string = "";
</script>

<div
  class="w-full min-h-[5rem] flex flex-col my-2 py-3 px-6 border-y border-slate-800 flex-shrink-0"
  in:scale|local={{ duration: 300 }}
>
  <div class="flex mb-4">
    <Avatar {dp} name={poster} />

    <div class="p-2">
      <h5 class="font-semibold text-lg mb-1">
        {poster}
        <span class="ml-2 font-semibold text-sm text-slate-600">{time}</span>
      </h5>
      <div class="flex flex-col flex-1">
        {#each post.split("\n") as line}
          <p>
            {line}
            {#if line.trim().length == 0}
              <br />
            {/if}
          </p>
        {/each}
      </div>
    </div>
  </div>

  <div class="w-full flex justify-between">
    <!-- <p class="card__comments">
      {showCommentStatement}
    </p> -->

    <p class="reactions" />
  </div>
</div>
