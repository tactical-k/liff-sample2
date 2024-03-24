<script>
  import liff from "@line/liff";

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
  }

  let promise = init();
</script>

<main>
  <div>
    { liff.getOS}
  </div>
  <div>
    { liff.getProfile }
  </div>
</main>

<style>
  main {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
