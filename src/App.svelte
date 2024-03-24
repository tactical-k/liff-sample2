<script>
  import { onMount } from 'svelte';
  import liff from "@line/liff";
  // import Textfield from '@smui/textfield';
  // import axios from 'axios';

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
  }

  let promise = init();

  let userName = '';
  let userUuid = '';
  const getUserLineProfile = async () => {
    try {
      const profile = await liff.getProfile();
      userName = profile.displayName;
      userUuid = profile.userId;
    } catch (err) {
      console.log("error", err);
    }
  }

  // let formData = '';


  // const handleSubmit = async () => {
  //   console.log('submit');
  //   try {
  //     const response = await axios.post(
  //       'https://huge-frog-75.hasura.app/v1/graphql',
  //       {
  //         query: `
  //           mutation InsertData($lineUuid: String!, $data: String!) {
  //             insert_user_one(object: { line_uuid: $lineUuid, data: $data }) {
  //               id
  //             }
  //           }
  //         `,
  //         variables: {
  //           lineUuid: userUuid,
  //           data: formData
  //         },
  //         headers: {
  //           'x-hasura-admin-secret': import.meta.env.HASURA_ADMIN_SECRET
  //         }
  //       }
  //     );
  //   } catch (error) {
  //     console.error(error);
  //   }
  // }

  onMount(async () => {
    await promise;
    await getUserLineProfile();
  });
</script>

<main>
  <!-- <div>
    <Textfield textarea bind:value={formData}></Textfield>
    <button on:click={handleSubmit}>送信</button>
  </div> -->
  {#await promise then}
    {#if liff.isLoggedIn()}
      {#if userName}
        <p>こんにちは。{userName}さん</p>
        <p>あなたの目標を教えて下さい。</p>
        <!-- <div>
          <Textfield textarea bind:value={formData}></Textfield>
          <button on:click={handleSubmit}>送信</button>
        </div> -->
        <p>ログイン済</p>
      {/if}
    {:else}
      <p>スマホアプリでLINEを開いてください</p>
    {/if}
  {:catch e}
    <p>LIFF init failed.</p>
    <p><code>{`${e}`}</code></p>
  {/await}
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
