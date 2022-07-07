<script context="module">
  import Textfield from "@smui/textfield";
  import HelperText from "@smui/textfield/helper-text";

  let phoneNumber = "";
  let smsMessage = "";

  export function generateSmsString() {
    return `smsto:${phoneNumber}:${smsMessage}`;
  }
</script>

<Textfield variant="filled" label="Phone number" bind:value={phoneNumber} type="tel" required>
  <HelperText slot="helper">The phone number of the recipient</HelperText>
</Textfield>
<Textfield variant="filled" label="SMS message" bind:value={smsMessage} style="margin-top: 12px;">
  <HelperText slot="helper">The SMS message</HelperText>
</Textfield>

<div style="margin-top: 12px;">
  <slot />
</div>
