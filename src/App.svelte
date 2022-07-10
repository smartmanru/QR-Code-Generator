<script>
  import { fly } from "svelte/transition";

  import Header from "./lib/Header.svelte";
  import WifiCode, { generateWifiString } from "./lib/WifiCode.svelte";
  import SmsCode, { generateSmsString } from "./lib/SmsCode.svelte";
  import QR from "./lib/QR.svelte";
  import QRSVG from "./lib/QRSVG.svelte";

  import Select, { Option } from "@smui/select";
  import Textfield from "@smui/textfield";
  import HelperText from "@smui/textfield/helper-text";
  import CharacterCounter from "@smui/textfield/character-counter";
  import LayoutGrid, { Cell } from "@smui/layout-grid";
  import Slider from "@smui/slider";
  import Card from "@smui/card";
  import Paper, { Title, Content } from "@smui/paper";
  import Tab, { Label } from "@smui/tab";
  import TabBar from "@smui/tab-bar";

  let size = 250;
  let ecl = "M";
  let active = "Text";
  let input = "";

  $: {
    if (active) {
      input = "";
    }
  }

  function generateWifiCode() {
    input = generateWifiString();
  }

  function generateSmsCode() {
    input = generateSmsString();
  }
</script>

<!-- https://materialdesignicons.com/ -->
<!-- https://materialui.co/colors/ -->
<!-- https://sveltematerialui.com/demo/layout-grid/ -->
<!-- https://www.youtube.com/watch?v=OyjZ7dezADw&t=1073s -->
<!-- https://github.com/zxing/zxing/wiki/Barcode-Contents -->

<Header />

<LayoutGrid>
  <Cell spanDevices={{ desktop: 3, tablet: 0, phone: 0 }} />
  <Cell spanDevices={{ desktop: 6, tablet: 12, phone: 12 }}>
    <Paper style="margin-bottom: 2em;" color="primary" variant="outlined">
      <Title>General QR-Code settings</Title>
      <Content>Size:</Content>
      <Slider bind:value={size} min={100} max={400} step={10} discrete tickMarks style="margin-left: 60px;" />
      <Select variant="filled" bind:value={ecl} label="Error correction level" style="min-width: 300px;">
        <Option value="L">Low, 7% redundant</Option>
        <Option value="M">Medium, 15% redundant</Option>
        <Option value="Q">Quartile, 25% redundant</Option>
        <Option value="H">High, 30% redundant</Option>
      </Select>
    </Paper>
    <TabBar tabs={["Text", "Wifi", "SMS"]} let:tab bind:active>
      <!-- Note: the `tab` property is required! -->
      <Tab {tab}>
        <Label>{tab}</Label>
      </Tab>
    </TabBar>
    <Card style="margin-bottom: 2em; padding: 2em;">
      {#if active === "Text"}
        <Textfield textarea input$maxlength={1000} spellcheck="false" label="Text for the QR-Code" style="min-height: 200px;" bind:value={input}>
          <HelperText slot="helper">The QR-Code is automatically updated with text changes</HelperText>
          <CharacterCounter slot="internalCounter">0 / 1000</CharacterCounter>
        </Textfield>
      {:else if active === "Wifi"}
        <WifiCode>
          <button on:click={generateWifiCode}>Generate Wifi QR-Code</button>
        </WifiCode>
      {:else}
        <SmsCode>
          <button on:click={generateSmsCode}>Generate SMS QR-Code</button>
        </SmsCode>
      {/if}
    </Card>
  </Cell>
  <Cell spanDevices={{ desktop: 3, tablet: 12, phone: 12 }} style="display: flex;">
    {#if input}
      <div style="margin: auto;" in:fly={{ y: 250, duration: 1000 }} out:fly={{ y: 250, duration: 500 }}>
        <!-- <QR codeValue={input} {size} /> -->
        <QRSVG codeValue={input} {size} {ecl} />
      </div>
    {:else}
      <!-- <p>placeholder</p> -->
    {/if}
  </Cell>
</LayoutGrid>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  * :global(.main2) {
    padding: 20px;
    margin: 20px;
  }

  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: unset;
    width: 300px;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: rgba(255, 62, 0, 0.2);
  }
</style>
