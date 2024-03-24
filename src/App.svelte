<script>
  import liff from "@line/liff";

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
  }

  async function getUserProfile() {
    await liff.getProfile()
  }

  let promise = init();

  let userProfile = getUserProfile();

  let os = liff.getOS();
  let userProfile = () => { liff.getProfile() 
    .then((profile) => {
      return {
        userId: profile.userId,
        displayName: profile.displayName
      }
    })
    .catch((err) => {
      console.error(err)
    });
    

  }
</script>

<main>
  <h1>create-liff-app</h1>
  {#await promise}
    <p>LIFF init...</p>
  {:then}
  <div>
    { os }
  </div>
  <div>
    {#if (liff.isLoggedIn()) }
      {#await userProfile}
        <p>読込中</p>
      {:then } 
        <div>
          {userProfile.userId}
        </div>
        <div>
          {userProfile.displayName}
        </div>
      {/await}
    {:else}
    ログインしてください
    {/if}
  </div>
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
