<!-- https://github.com/SumiMakito/Awesome-qr.js -->
<script>
  import { AwesomeQR } from "awesome-qr";

  export let codeValue;
  export let size = 300;

  async function getAwesomeQR() {
    const buffer = await new AwesomeQR({
      text: codeValue,
      size: size,
    }).draw();
    return buffer;
  }

  let promise;

  $: {
    if (codeValue) {
      promise = getAwesomeQR();
    }
  }
</script>

{#await promise}
  <p>...waiting</p>
{:then buffer}
  <img src={buffer} alt={codeValue} width={size} height={size} />
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}
