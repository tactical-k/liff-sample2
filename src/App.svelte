<script>
  import { onMount } from 'svelte';
  import liff from "@line/liff";
  import Textfield from '@smui/textfield';
  import {useMutation} from '@apollo/client';
  import { gql } from '@apollo/client/core';

  async function init() {
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
  }

  let promise = init();

  let user;
  const getUserLineProfile = async () => {
    try {
      const profile = await liff.getProfile();
      user.name = profile.displayName;
      user.uuid = profile.userId;
    } catch (err) {
      console.log("error", err);
    }
  }

  let formData = '';

  const INSERT_DATA = gql`
    mutation MyMutation($lineUuid: String!, $data: String!) {
      insert_user_one(object: {data: $data, line_uuid: $lineUuid}) {
        id
      }
    }
  `;

  const [insertData] = useMutation(INSERT_DATA, {
    context: {
      uri: 'https://huge-frog-75.hasura.app/v1/graphql',
      headers: {
        'x-hasura-admin-secret': import.meta.env.HASURA_ADMIN_SECRET
      }
    }
  });

  const handleSubmit = async () => {
    try {
      await insertData({
        variables: {
          lineUuid: user.id,
          data: formData
        }
      });
    } catch(error) {
      console.error(error);
    }
  }

  onMount(async () => {
    await promise;
    await getUserLineProfile();
  });
</script>

<main>
  {#await promise then}
    {#if liff.isLoggedIn()}
      {#if user}
        <p>こんにちは。{user.name}さん</p>
        <p>あなたの目標を教えて下さい。</p>
        <div>
          <Textfield textarea bind:value={formData}></Textfield>
          <button on:click={handleSubmit}>送信</button>
        </div>
      {/if}
        <p>ログイン済</p>
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
