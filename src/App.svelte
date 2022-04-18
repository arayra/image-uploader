<script lang="ts">
  import { onMount } from "svelte";

  let state = "beforeUpload";
  let url = "https://arayra-image-uploader.herokuapp.com/";
  let textLink = "https://images.yourdomain.com/1496950866446-325.png";
  let textLinkElement;
  let imageToUpload;
  let imageLink;

  onMount(() => {
    textLinkElement = textLinkElement; // https://svelte.dev/tutorial/bind-this
  });

  const uploadImage = (image) => {
    let formData = new FormData();

    formData.append("image", image);
    fetch(url, {
      method: "POST",
      body: formData,
    })
      .then((res) => res.json())
      .then((data) => {
        state = "afterUpload";
        imageLink = url + data.path;
        textLink = imageLink;
      })
      .catch((err) => console.log(err));
  };

  const retrievingImage = (ev) => {
    state = "loading";
    let image = ev.dataTransfer.files[0];
    uploadImage(image);
  };

  const copyImageLink = () => {
    textLinkElement.select();
    textLinkElement.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(textLinkElement.value);
  };

  $: if (imageToUpload) {
    state = "loading";
    let image = imageToUpload[0];
    uploadImage(image);
  }
</script>

<main>
  <div class="wrap">
    {#if state === "beforeUpload"}
      <h1 class="extra-margin">Upload Your Image</h1>
      <p class="extra-margin">File should be Jpeg, Png,...</p>
      <div
        class="drag-and-drop extra-margin"
        on:drop|preventDefault={retrievingImage}
        on:dragover|preventDefault
      >
        <img src="../src/assets/image.svg" alt="Drag & Drop SVG" />
        <span class="text-inside">Drag & Drop your image here</span>
      </div>
      <span class="text-outside extra-margin">Or</span>
      <input
        type="file"
        name="image"
        id="image"
        bind:files={imageToUpload}
        accept=".jpg, .jpeg, .png"
      />
      <button class="choose-a-file extra-margin">
        <label for="image">Choose a file</label>
      </button>
    {/if}
    {#if state === "loading"}
      <h1 class="extra-margin">Uploading...</h1>
      <div class="loading-bar extra-margin">
        <div class="progress" />
      </div>
    {/if}
    {#if state === "afterUpload"}
      <img
        class="circle-check extra-margin"
        src="../src/assets/circle-check.svg"
        alt="Circle Check SVG"
      />
      <h1 class="extra-margin">Uploaded Successfully!</h1>
      <div class="image-result extra-margin">
        <img src={imageLink} alt="Uploaded File" loading="lazy" />
      </div>
      <div class="image-link extra-margin">
        <input
          type="text"
          class="text-link"
          bind:value={textLink}
          bind:this={textLinkElement}
          readonly
        />
        <button class="copy-link" on:click={copyImageLink}>Copy Link</button>
      </div>
    {/if}
  </div>
</main>

<style lang="scss">
  @use "./sass/_variables.scss" as *;

  :global {
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
  }

  main {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
    background-color: $background-secondary;

    .wrap {
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      align-items: center;
      width: 85%;
      max-width: 402px;
      background-color: $background-primary;
      box-shadow: 0px 4px 12px 0px #0000001a;
      border-radius: $border-radius-primary;
      padding: 25px;

      h1 {
        font-family: "Poppins";
        font-weight: 500;
        font-size: 18px;
        letter-spacing: -0.035em;
        color: #4f4f4f;
        height: 26.99px;
        line-height: 27px;
      }

      p {
        font-family: "Poppins";
        font-weight: 500;
        font-size: 10px;
        letter-spacing: -0.035em;
        color: #828282;
        line-height: 15px;
      }

      .drag-and-drop {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        width: 85%;
        max-width: 338px;
        height: 218.9px;
        background-color: $background-tertiary;
        border: 2px dashed #bdbdbd;
        border-radius: $border-radius-primary;
        padding: 25px 0;

        span.text-inside {
          font-family: "Poppins";
          font-weight: 500;
          font-size: 12px;
          letter-spacing: -0.035em;
          color: #bdbdbd;
        }
      }

      span.text-outside {
        font-family: "Poppins";
        font-style: normal;
        font-weight: 500;
        font-size: 12px;
        line-height: 18px;
        text-align: center;
        letter-spacing: -0.035em;
        color: #bdbdbd;
      }

      input[type="file"] {
        display: none;
      }

      button.choose-a-file {
        width: 101px;
        height: 31.98px;
        font-family: "Noto Sans";
        font-weight: 500;
        font-size: 12px;
        letter-spacing: -0.035em;
        color: #ffffff;
        border: 0px;
        border-radius: $border-radius-secondary;

        label {
          display: flex;
          justify-content: center;
          align-items: center;
          width: inherit;
          height: inherit;
          background-color: #2f80ed;
          border-radius: inherit;
          cursor: pointer;
        }
      }
    }

    .loading-bar {
      width: 100%;
      height: 6px;
      margin: 0 auto;
      background-color: #f2f2f2;
      border-radius: $border-radius-secondary;
      position: relative;

      .progress {
        position: absolute;
        border-radius: inherit;
        top: 0;
        right: 100%;
        bottom: 0;
        left: 0;
        background-color: #2f80ed;
        width: 0;
        animation: moving-bar 2s linear infinite;
      }
    }

    img.circle-check {
      width: 35px;
      filter: invert(57%) sepia(10%) saturate(3133%) hue-rotate(93deg)
        brightness(80%) contrast(81%);
    }

    .image-result {
      width: 100%;
      height: 224.4px;
      background-color: #d4d4d4;
      border-radius: $border-radius-primary;

      img {
        width: inherit;
        height: inherit;
        border-radius: inherit;
        object-fit: cover;
      }
    }

    .image-link {
      display: flex;
      justify-content: space-between;
      align-items: center;
      min-width: 90%;
      height: 34px;
      background-color: #f6f8fb;
      border: 1px solid #e0e0e0;
      border-radius: 12px;
      padding: 0 0 0 7px;

      input.text-link {
        font-family: "Poppins";
        font-style: normal;
        font-weight: 500;
        font-size: 8px;
        line-height: 12px;
        width: 100%;
        letter-spacing: -0.035em;
        border: transparent;
        outline: none;
      }

      button.copy-link {
        width: 74px;
        height: 30px;
        color: #ffffff;
        background-color: #2f80ed;
        border: 0px;
        border-radius: 8px;
        font-family: "Poppins";
        font-weight: 500;
        font-size: 8px;
        text-align: center;
        letter-spacing: -0.035em;
        cursor: pointer;
      }
    }
  }

  .extra-margin {
    margin: 14px 0;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }

    p {
      max-width: none;
    }
  }

  @keyframes moving-bar {
    0% {
      left: 0%;
      right: 100%;
      width: 0%;
    }
    10% {
      left: 0%;
      right: 75%;
      width: 25%;
    }
    90% {
      right: 0%;
      left: 75%;
      width: 25%;
    }
    100% {
      left: 100%;
      right: 0%;
      width: 0%;
    }
  }
</style>
