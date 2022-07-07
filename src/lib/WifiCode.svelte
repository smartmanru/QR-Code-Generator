<script context="module">
  import Textfield from "@smui/textfield";
  import HelperText from "@smui/textfield/helper-text";
  import Checkbox from "@smui/checkbox";
  import FormField from "@smui/form-field";
  import Select, { Option } from "@smui/select";

  let ssid = "";
  let password = "";
  let security = "WPA";
  let isHidden = false;

  export function generateWifiString() {
    // WIFI:T:WPA;S:mynetwork;P:mypass;;
    let code = `WIFI:T:${security};S:${ssid};P:${password};`;

    if (isHidden) {
      code += "H:true;";
    } else {
      code += ";";
    }
    return code;
  }
</script>

<Textfield variant="filled" label="SSID" bind:value={ssid}>
  <HelperText slot="helper">The name of the Wifi network</HelperText>
</Textfield>
<Textfield variant="filled" label="Password" bind:value={password} style="margin-top: 12px;">
  <HelperText slot="helper">The password of the Wifi network</HelperText>
</Textfield>

<Select variant="filled" bind:value={security} label="Security" style="margin-top: 12px;">
  <Option value="WPA">WPA</Option>
  <Option value="WEP">WEP</Option>
  <Option value="nopass">No password</Option>
</Select>

<FormField style="margin-top: 12px;">
  <Checkbox bind:isHidden touch />
  <span slot="label">Hidden?</span>
</FormField>

<slot />
