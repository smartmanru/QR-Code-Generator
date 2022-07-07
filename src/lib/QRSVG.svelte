<!-- https://github.com/papnkukn/qrcode-svg -->
<script>
  import QRCode from "qrcode-svg";

  export let codeValue;
  export let size = 300;
  export let ecl = "M";
  let svgEncoded;

  function generateQrCode() {
    var qrcode = new QRCode({
      content: codeValue,
      padding: 2,
      width: size,
      height: size,
      color: "#000000",
      background: "#ffffff",
      ecl: ecl,
      xmlDeclaration: false,
    });
    var svg = qrcode.svg();
    svgEncoded = `data:image/svg+xml,${encodeURIComponent(svg)}`;
  }

  $: {
    if (codeValue && ecl) {
      generateQrCode();
    }
  }
</script>

<img src={svgEncoded} alt={codeValue} width={size} height={size} />
