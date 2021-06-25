<script>
  import TodoItem from "./TodoItem.svelte";

  import { db } from "./firebase";
  import { collectionData } from "rxfire/firestore";
  import { startWith } from "rxjs/operators";
  import { slide, scale } from "svelte/transition";
  import { flip } from 'svelte/animate'

  export let uid;

  let text;

  const query = db
    .collection("todos")
    .where("uid", "==", uid)
    .orderBy("created");
  const todos = collectionData(query, "id").pipe(startWith([]));

  $: console.log(todos)

  function add(event) {
    if (event.key === "Enter" || event.target.id === "add-task")
      if (text !== "") {
        db.collection("todos").add({
          uid,
          text,
          complete: false,
          mark: false,
          created: Date.now(),
        });
        text = "";
      }
  }

  function updateState(event) {
    const { id, newState } = event.detail;
    db.collection("todos").doc(id).update({ complete: newState });
  }

  function updateMark(event) {
    const { id, newMark } = event.detail;
    db.collection("todos").doc(id).update({ mark: newMark });
  }

  function removeItem(event) {
    const { id } = event.detail;
    db.collection("todos").doc(id).delete();
  }
</script>

<div class="max-w-xl bg-white rounded-lg shadow-lg overflow-hidden" transition:scale={{ delay: 900 }}>
  <div class="relative">
    <img
      src="https://images.unsplash.com/photo-1500099817043-86d46000d58f?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=800&h=250&q=80"
      class="object-cover"
      alt=""
      transition:scale={{ delay: 1300 }}
    />
    <div
      class="absolute p-4 inset-0 flex flex-col justify-end bg-gradient-to-b from-transparent to-gray-900 backdrop backdrop-blur-5 text-white"
      transition:scale={{ delay: 1700 }}
    >
      <h3 class="font-bold" transition:scale={{ delay: 1600 }}>Todo list</h3>
      <span class="opacity-70" transition:scale={{ delay: 1800 }}>Albumtitle</span>
    </div>
  </div>
  <div>
    <div class="relative h-1 bg-gray-200" transition:scale={{ delay: 1800 }}>
      <div
        class="absolute h-full w-1/2 bg-green-500 flex items-center justify-end"
      >
        <div class="rounded-full w-3 h-3 bg-white shadow" />
      </div>
    </div>
  </div>
  <div
    class="flex justify-between text-xs font-semibold text-gray-500 px-4 py-2"
  >
    <div transition:slide={{ delay: 1900 }}>1:50</div>
    <div class="flex space-x-3 p-2">
      <button class="focus:outline-none" transition:scale={{ delay: 2200 }}>
        <svg
          class="w-4 h-4"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          ><polygon points="19 20 9 12 19 4 19 20" /><line
            x1="5"
            y1="19"
            x2="5"
            y2="5"
          /></svg
        >
      </button>
      <button
        class="rounded-full w-8 h-8 flex items-center justify-center pl-0.5 ring-2 ring-gray-100 focus:outline-none"
        transition:scale={{ delay: 2100 }}
      >
        <svg
          class="w-5 h-5"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"><polygon points="5 3 19 12 5 21 5 3" /></svg
        >
      </button>
      <button class="focus:outline-none" transition:scale={{ delay: 2200 }}>
        <svg
          class="w-4 h-4"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          ><polygon points="5 4 15 12 5 20 5 4" /><line
            x1="19"
            y1="5"
            x2="19"
            y2="19"
          /></svg
        >
      </button>
    </div>
    <div transition:slide={{ delay: 1900 }}>3:00</div>
  </div>
  <ul class="text-xs sm:text-base divide-y border-t cursor-default">
    {#each $todos as todo (todo.id)}
      <div transition:slide animate:flip={{duration: 500}}>
        <TodoItem
          {...todo}
          on:remove={removeItem}
          on:toggleState={updateState}
          on:toggleMark={updateMark}
        />
      </div>
    {/each}

    <li class="flex items-center space-x-3 hover:bg-gray-100">
      <button
        class="p-3 hover:bg-green-500 group focus:outline-none"
        on:click={() => (text = "")}
      >
        <svg
          class="w-4 h-4 group-hover:text-white"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"><polygon points="5 3 19 12 5 21 5 3" /></svg
        >
      </button>
      <input
        type="text"
        placeholder="Add some task..."
        class="flex-1 hover:bg-gray-100"
        bind:value={text}
        on:keyup={add}
      />
      <button
        class="focus:outline-none pr-4 group"
        id="add-task"
        on:click={add}
      >
        <svg
          id="add-task"
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-plus-circle"
          viewBox="0 0 16 16"
        >
          <path
            id="add-task"
            d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
          />
          <path
            id="add-task"
            d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
          />
        </svg>
      </button>
    </li>
  </ul>
</div>
