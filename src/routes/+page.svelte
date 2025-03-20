<script lang="ts">
  import HologramFace from "$lib/components/HologramFace.svelte";
  import { writable } from "svelte/store";

  const messages = writable<
    Array<{ role: "user" | "assistant"; content: string }>
  >([]);
  const currentEmotion = writable<
    "happy" | "sad" | "surprised" | "thoughtful" | "confident"
  >("happy");
  let userInput = "";

  async function handleSubmit() {
    if (!userInput.trim()) return;

    // Add user message
    messages.update((msgs) => [...msgs, { role: "user", content: userInput }]);
    const userMessage = userInput;
    userInput = "";

    try {
      // TODO: Implement OpenAI API call here
      // For now, we'll just simulate a response
      const response =
        "I'm processing your message and adjusting my emotional state accordingly.";

      // Add assistant message
      messages.update((msgs) => [
        ...msgs,
        { role: "assistant", content: response },
      ]);

      // Simulate emotion change
      const emotions: Array<
        "happy" | "sad" | "surprised" | "thoughtful" | "confident"
      > = ["happy", "sad", "surprised", "thoughtful", "confident"];
      currentEmotion.set(emotions[Math.floor(Math.random() * emotions.length)]);
    } catch (error) {
      console.error("Error processing message:", error);
    }
  }
</script>

<div class="container mx-auto px-4 py-8">
  <h1 class="text-4xl font-bold text-center mb-8">AI Hologram Agent</h1>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
    <!-- Hologram Display -->
    <div class="bg-gray-800 rounded-lg p-4">
      <HologramFace emotion={$currentEmotion} />
    </div>

    <!-- Chat Interface -->
    <div class="bg-white rounded-lg shadow-lg p-4">
      <div class="h-[400px] overflow-y-auto mb-4 space-y-4">
        {#each $messages as message}
          <div
            class="flex {message.role === 'user'
              ? 'justify-end'
              : 'justify-start'}"
          >
            <div
              class="max-w-[80%] rounded-lg p-3 {message.role === 'user'
                ? 'bg-blue-500 text-white'
                : 'bg-gray-200'}"
            >
              {message.content}
            </div>
          </div>
        {/each}
      </div>

      <form on:submit|preventDefault={handleSubmit} class="flex gap-2">
        <input
          type="text"
          bind:value={userInput}
          placeholder="Type your message..."
          class="flex-1 p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button
          type="submit"
          class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition-colors"
        >
          Send
        </button>
      </form>
    </div>
  </div>
</div>

<style>
  :global(body) {
    background-color: #f3f4f6;
  }
</style>
