<script lang="ts">
  import { changeVote, type Option, type Poll } from "./data/polls";
  import PollVoteBar from "./poll_vote_bar.svelte";
  import FaIcon from "../lib/faIcon.svelte";
  import { slide } from "svelte/transition";

  export let option: Option;
  export let poll: Poll;

  const castVote = async (oid: string) => {
    //update the vote count for the option
    try {
      let success = await changeVote(poll.id, oid, true);
      if (success) {
        poll.voted_option = oid;
      }
    } catch (e) {
      console.log(e);
    }
  };

  const removeVote = async (oid: string) => {
    try {
      let success = await changeVote(poll.id, oid, false);
      if (success) {
        poll.voted_option = "-1";
      }
    } catch (e) {
      console.log(e);
    }
  };

  let isdescriptionexpanded: boolean = false;
</script>

<div>
  <div
    class="grid grid-cols-10 h-full w-full items-center hover:bg-slate-600 rounded-lg p-2 hover:cursor-pointer"
    on:click={() => {
      isdescriptionexpanded = !isdescriptionexpanded;
    }}
  >
    <span
      class="text-xl font-medium text-blue-600 dark:text-blue-500 col-span-2 truncate"
      title={option.description}>{option.title}</span
    >
    <div class="col-span-6 px-1">
      <PollVoteBar bind:percent={option.width} />
    </div>

    <span
      class="text-xl font-medium text-blue-600 dark:text-blue-500 mx-auto col-span-1"
      >{option.width}% ({option.vote_count})</span
    >
    {#if poll.voted_option == "-1"}
      <button
        class="h-10 w-10 shadow-xl text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-full lg:ml-5 text-sm mr-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 focus:outline-none dark:focus:ring-green-800 disabled:cursor-not-allowed disabled:bg-green-700 col-span-1"
        type="button"
        on:click|preventDefault|stopPropagation={() => castVote(option.id)}
      >
        <FaIcon type="regular" icon="chevron-up" className="text-sm" />
      </button>
    {:else if poll.voted_option == option.id}
      <button
        class="h-10 w-10 shadow-xl text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-full lg:ml-5 text-sm mr-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 focus:outline-none dark:focus:ring-red-800 disabled:cursor-not-allowed disabled:bg-red-700 col-span-1"
        type="button"
        disabled={poll.voted_option != option.id}
        on:click|preventDefault|stopPropagation={() => removeVote(option.id)}
      >
        <FaIcon type="regular" icon="chevron-down" className="text-sm" />
      </button>
    {:else}
      <div class="h-10" />
    {/if}
  </div>
  {#if isdescriptionexpanded}
    <div
      class="text-lg font-medium text-black dark:text-white my-3 whitespace-normal mr-12"
      transition:slide|local
      title={option.description}
    >
      {option.description}
    </div>
  {/if}
</div>
