<script>
	const RPI_URL = "http://192.168.0.223:8080";
	export let data = {};
  export async function handleChange(event, color) {
    const { value } = event.target;
    const response = await fetch(`${RPI_URL}/${color}?value=${value}`);
    if (response.ok) {
			const responseData = await response.json();
			data[responseData.color] = responseData.value;
    }
  }
</script>

<main>
  <div class="blue">
		<span class="label">{data.blue || 0}</span>
    <input
      type="range"
      min="0"
      max="1"
      step="0.01"
      on:change={(e) => handleChange(e, "blue")}
    /><br />
  </div>
  <div class="green">
		<span class="label">{data.blue || 0}</span>
    <input
      type="range"
      min="0"
      max="1"
      step="0.01"
      on:change={(e) => handleChange(e, "green")}
    /><br />
  </div>
  <div class="red">
		<span class="label">{data.blue || 0}</span>
    <input
      type="range"
      min="0"
      max="1"
      step="0.01"
      on:change={(e) => handleChange(e, "red")}
    />
  </div>
</main>

<style>
</style>
