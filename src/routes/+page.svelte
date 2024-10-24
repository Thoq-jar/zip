<script lang="ts">
  import JSZip from 'jszip';

  let status = '';
  let generating = false;
  let powerfulComputer = false;

  function generateRandomString(length: number): string {
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    let result = '';
    for (let i = 0; i < length; i++) {
      result += characters.charAt(Math.floor(Math.random() * characters.length));
    }
    return result;
  }

  async function generate() {
    generating = true;
    status = 'Generating...';
    const zip = new JSZip();
    const fileCount = 3000;
    const fileSize = powerfulComputer ? 50000 : 10000;

    for (let i = 0; i < fileCount; i++) {
      const fileContent = generateRandomString(fileSize);
      zip.file(`file${i + 1}.txt`, fileContent);
      status = `Created file ${i + 1} of ${fileCount}`;
      await new Promise(resolve => setTimeout(resolve, 10));
    }

    const content = await zip.generateAsync({ type: "blob" });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(content);
    link.download = "alakazam.zip";
    link.click();
    status = "Ready to download!";
    generating = false;
  }
</script>

<main>
  <label>
    <input type="checkbox" bind:checked={powerfulComputer} />
    I Have A Powerful Computer
  </label>
  <button on:click={generate} disabled={generating}>
    {generating ? 'Generating...' : 'Generate'}
  </button>
  <div>{status}</div>
</main>

<style>
</style>