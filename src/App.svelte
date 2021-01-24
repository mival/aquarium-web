<script>
  const RPI_URL = "http://192.168.0.223:8080";
  export let data = {};
  export let colors = [];
  export let state = 0;

  async function loadData() {
    const response = await fetch(`${RPI_URL}/data`);
    if (response.ok) {
      const responseData = await response.json();
      data = responseData;
    }
  }

  loadData();

  async function loadConfig() {
    const response = await fetch(`${RPI_URL}/colors`);
    if (response.ok) {
      const responseData = await response.json();
      colors = responseData;
    }
  }

  loadConfig();

  export async function handleChange(event, color) {
    const { value } = event.target;
    const response = await fetch(`${RPI_URL}/set/${color}`, {
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      },
      method: 'post',
      body: JSON.stringify({color, value})
    });
    if (response.ok) {
      const responseData = await response.json();
      data[responseData.color] = responseData.value;
    }
  }

  export async function togglePower(state = 0) {
    const response = await fetch(`${RPI_URL}/turn-${state ? 'on' : 'off'}`, {
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      },
      method: 'post'
    });

    if (response.ok) {
      data = await response.json();
    }
  }
</script>

<main>
  {#each colors as color}
    <div class={color}>
      <span class="label">{data[color] || 0}</span>
      <input
        type="range"
        min="0"
        max="1"
        step="0.01"
        value={data[color]}
        on:change={(e) => handleChange(e, color)}
      /><br />
    </div>
  {/each}
  <button on:click={() => togglePower(state = !state)}>Toggle</button>
</main>

<style>
</style>
