<script lang="ts">
  import { Client } from "../TCoreLib.JS/src/index";

  let firstName = "";

  let submitted = false;

  const tcore = new Client("http://192.168.2.33:8080");

  let tcoreLoaded = false;
  tcore.InitRest().then(() => (tcoreLoaded = true));

  const submit = async (e: Event) => {
    e.preventDefault();

    if (tcoreLoaded && firstName !== "") {
      try {
        tcore.sendRequest({
          hook: "name",
          function: "save_name",
          action: "null",
          params: [firstName],
        });
      } catch (e) {
        console.log(e);
      }
    }

    console.log(firstName);

    submitted = true;
  };
</script>

<main>
  {#if !submitted}
    <!-- form from https://codepen.io/atunnecliffe/pen/gpKzQw 💖 -->
    <form on:submit={submit}>
      <input
        type="text"
        name="name"
        class="question"
        id="name"
        required
        autocomplete="off"
        bind:value={firstName}
      />
      <label for="name"><span>What's your name?</span></label>

      {#if firstName !== ""}
        <input type="submit" value="Submit!" />
      {/if}
    </form>
  {:else}
    <h1>Demo profile created</h1>
    <a href="/">sign in with this profile here</a>
  {/if}
</main>

<style>
  main {
    display: grid;
    place-items: center;
    height: 100vh;
  }

  form {
    display: flex;
    flex-direction: column;
    height: 50%;
    gap: 6px;
  }

  input,
  span,
  label {
    display: block;
    margin: 10px;
    padding: 5px;
    border: none;
    font-size: 22px;
  }

  input:focus {
    outline: 0;
  }
  input.question {
    font-size: 48px;
    font-weight: 300;
    border-radius: 2px;
    margin: 0;
    border: none;
    width: 80%;
    background: rgba(0, 0, 0, 0);
    transition: padding-top 0.2s ease, margin-top 0.2s ease;
    overflow-x: hidden;
  }

  input.question + label {
    display: block;
    position: relative;
    white-space: nowrap;
    padding: 0;
    margin: 0;
    width: 10%;
    border-top: 3px solid rgb(215, 77, 77);
    -webkit-transition: width 0.4s ease;
    transition: width 0.4s ease;
    height: 0px;
  }

  input.question:focus + label {
    width: 80%;
  }

  input.question:focus,
  input.question:valid {
    padding-top: 35px;
  }

  input.question:focus + label > span,
  input.question:valid + label > span {
    top: -100px;
    font-size: 22px;
    color: #fff;
  }

  input.question:valid + label {
    border-color: rgb(78, 239, 78);
  }

  input.question:invalid {
    box-shadow: none;
  }

  input.question + label > span {
    font-weight: 300;
    margin: 0;
    position: absolute;
    color: #eee;
    font-size: 48px;
    top: -66px;
    left: 0px;
    z-index: -1;
    -webkit-transition: top 0.2s ease, font-size 0.2s ease, color 0.2s ease;
    transition: top 0.2s ease, font-size 0.2s ease, color 0.2s ease;
  }

  input[type="submit"] {
    -webkit-transition: opacity 0.2s ease, background 0.2s ease;
    transition: opacity 0.2s ease, background 0.2s ease;
    display: block;
    opacity: 0;
    margin: 10px 0 0 0;
    padding: 10px;
    cursor: pointer;
    width: min-content;
    background-color: white;
    color: black;
  }

  input[type="submit"]:hover {
    background: rgb(220, 220, 220);
  }

  input[type="submit"]:active {
    background: rgb(220, 220, 220);
  }

  input.question:valid ~ input[type="submit"] {
    -webkit-animation: appear 1s forwards;
    animation: appear 1s forwards;
  }

  input.question:invalid ~ input[type="submit"] {
    display: none;
  }

  @-webkit-keyframes appear {
    100% {
      opacity: 1;
    }
  }

  @keyframes appear {
    100% {
      opacity: 1;
    }
  }
</style>
