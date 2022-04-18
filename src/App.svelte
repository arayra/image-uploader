<script lang="ts">
  let state = "afterUpload"
  let textLink = "https://images.yourdomain.com/1496950866446-325.png"

  if (textLink.length > 60) textLink = textLink.substring(0, 60) + "..."
</script>

<main>
  <div class="wrap">
    {#if state === "beforeUpload"}
      <h1 class="extra-margin">Upload Your Image</h1>
      <p class="extra-margin">File should be Jpeg, Png,...</p>
      <div class="drag-and-drop extra-margin">
        <img src="../src/assets/image.svg" alt="Drag & Drop SVG">
        <span class="text-inside">Drag & Drop your image here</span>
      </div>
      <span class="text-outside extra-margin">Or</span>
      <button class="choose-a-file extra-margin">Choose a file</button>
    {/if}
    {#if state === 'loading'}
      <h1 class="extra-margin">Uploading...</h1>
      <div class="loading-bar extra-margin">
        <div class="progress"></div>
      </div>
    {/if}
    {#if state === 'afterUpload'}
      <img class="circle-check extra-margin" src="../src/assets/circle-check.svg" alt="Circle Check SVG">
      <h1 class="extra-margin">Uploaded Successfully!</h1>
      <div class="image-result extra-margin">

      </div>
      <div class="image-link extra-margin">
        <input type="text" class="text-link" bind:value={textLink} readonly>
        <button class="copy-link">Copy Link</button>
      </div>
    {/if}
  </div>
</main>

<style lang="scss">
  @use './sass/_variables.scss' as *;

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
      box-shadow: 0px 4px 12px 0px #0000001A;
      border-radius: $border-radius-primary;
      padding: 25px;

      h1 {
        font-family: 'Poppins';
        font-weight: 500;
        font-size: 18px;
        letter-spacing: -0.035em;
        color: #4F4F4F;
        height: 26.99px;
        line-height: 27px;
      }

      p {
        font-family: 'Poppins';
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
        border: 2px dashed #BDBDBD;
        border-radius: $border-radius-primary;
        padding: 25px 0;

        span.text-inside {
          font-family: 'Poppins';
          font-weight: 500;
          font-size: 12px;
          letter-spacing: -0.035em;
          color: #BDBDBD;
        }
      }

      span.text-outside {
        font-family: 'Poppins';
        font-style: normal;
        font-weight: 500;
        font-size: 12px;
        line-height: 18px;
        text-align: center;
        letter-spacing: -0.035em;
        color: #BDBDBD;
      }

      button.choose-a-file {
        width: 101px;
        height: 31.98px;
        background-color: #2F80ED;
        font-family: 'Noto Sans';
        font-weight: 500;
        font-size: 12px;
        letter-spacing: -0.035em;
        color: #FFFFFF;
        border: 0px;
        border-radius: $border-radius-secondary;
        cursor: pointer;
      }
    }

    .loading-bar {
      width: 100%;
      height: 6px; 
      margin: 0 auto;
      background-color: #F2F2F2;
      border-radius: $border-radius-secondary;
      position: relative;

      .progress {
        position: absolute;
        border-radius: inherit;
        top: 0;
        right: 100%;
        bottom: 0;
        left: 0;
        background-color: #2F80ED; 
        width: 0;
        animation: moving-bar 2s linear infinite;
      }
    }

    img.circle-check {
      width: 35px;
      filter: invert(57%) sepia(10%) saturate(3133%) hue-rotate(93deg) brightness(80%) contrast(81%);
    }

    .image-result {
      width: 100%;
      height: 224.4px;
      background-color: #d4d4d4;
      border-radius: $border-radius-primary;
    }

    .image-link {
      display: flex;
      justify-content: space-between;
      align-items: center;
      min-width: 90%;
      height: 34px;
      background-color: #F6F8FB;
      border: 1px solid #E0E0E0;
      border-radius: 12px;
      padding: 0 0 0 7px;

      input.text-link {
        font-family: 'Poppins';
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
        color: #FFFFFF;
        background-color: #2F80ED;
        border: 0px;
        border-radius: 8px;
        font-family: 'Poppins';
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
      left:0%;
      right:100%;
      width:0%;
    }
    10% {
      left:0%;
      right:75%;
      width:25%;
    }
    90% {
      right:0%;
      left:75%;
      width:25%;
    }
    100% {
      left:100%;
      right:0%;
      width:0%;
    }
  }
</style>
