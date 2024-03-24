<script>
  import liff from "@line/liff";

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
  }

  let promise = init();

  let userLineProfile = () => {
    liff.getProfile()
      .then((profile) => {
        return profile.displayName;
      });
  } 
</script>

<main>
  <h1>create-liff-app</h1>
  {#await promise}
    <p>LIFF init...</p>
  {:then}
    <p>LIFF init successed.</p>
    {#await userLineProfile}
      <p>読込中</p>
    {:then userName} 
      <p>読み込み完了</p>
      <p>{userName}</p>
    {/await}
    <p>
      {#if liff.isLoggedIn()}
        <p>ログイン済</p>
      {:else}
        <p>ログイン前</p>
      {/if}

    </p>
  {:catch e}
    <p>LIFF init failed.</p>
    <p><code>{`${e}`}</code></p>
  {/await}
  <a href="https://developers.line.biz/ja/docs/liff/" target="_blank" rel="noreferrer">
    LIFF Documentation
  </a>
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
