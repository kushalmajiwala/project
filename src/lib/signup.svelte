<script>
  import { onMount } from "svelte";
  import axios from "axios";
  let togglebtn = "bi bi-eye";
  let passtype = "password";
  let username = "";
  let email = "";
  let password = "";
  let rpassword = "";
  function toggle() {
    if (togglebtn === "bi bi-eye-slash") {
      togglebtn = "bi bi-eye";
      passtype = "password";
    } else {
      togglebtn = "bi bi-eye-slash";
      passtype = "text";
    }
  }
  function validate() {}
  onMount(async () => {});
</script>

<main>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Sign Up Form by Colorlib</title>
    </head>
    <body>
      <div class="main">
        <section class="signup">
          <div class="container">
            <div class="signup-content">
              <h2 class="form-title">Create account</h2>
              <div class="form-group">
                <input
                  type="text"
                  class="usernameicon form-input"
                  name="name"
                  id="name"
                  placeholder="Username"
                  bind:value={username}
                />
              </div>
              <div class="form-group">
                <input
                  type="email"
                  class="emailicon form-input"
                  name="email"
                  id="email"
                  placeholder="Email"
                  bind:value={email}
                />
              </div>
              <div class="form-group">
                {#if passtype === "password"}
                  <input
                    type="password"
                    class="passwordicon form-input"
                    placeholder="Password"
                    bind:value={password}
                  />
                {:else}
                  <input
                    type="text"
                    class="passwordicon form-input"
                    placeholder="Password"
                    bind:value={password}
                  />
                {/if}
                <i
                  style="font-size: 20px;"
                  on:click={toggle}
                  class="{togglebtn} field-icon"
                />
              </div>
              <div class="form-group">
                <input
                  type="password"
                  class="confirmpasswordicon form-input"
                  name="re_password"
                  id="re_password"
                  placeholder="Confirm password"
                  bind:value={rpassword}
                />
              </div>
              <div class="form-group">
                <input
                  type="submit"
                  name="submit"
                  id="submit"
                  class="form-submit"
                  value="Sign up"
                  on:click={validate}
                />
              </div>
            </div>
          </div>
        </section>
      </div>
    </body>
  </html>
</main>

<style>
  .usernameicon {
    background-image: url('../assets/images/usernameIcon.png');
  }
  .emailicon {
    background-image: url('../assets/images/mailIcon.png');
  }
  .passwordicon {
    background-image: url('../assets/images/passwordIcon.png');
  }
  .confirmpasswordicon {
    background-image: url('../assets/images/confirmPasswordIcon.png');
  }
  @font-face {
    font-family: "Montserrat";
    font-style: normal;
    font-weight: 400;
    src: url("../fonts/montserrat/Montserrat-Regular.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: italic;
    font-weight: 400;
    src: url("../fonts/montserrat/Montserrat-Italic.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: normal;
    font-weight: 500;
    src: url("../fonts/montserrat/Montserrat-Medium.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: normal;
    font-weight: 600;
    src: url("../fonts/montserrat/Montserrat-SemiBold.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: normal;
    font-weight: 700;
    src: url("../fonts/montserrat/Montserrat-Bold.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: italic;
    font-weight: 700;
    src: url("../fonts/montserrat/Montserrat-BoldItalic.ttf");
  }
  @font-face {
    font-family: "Montserrat";
    font-style: italic;
    font-weight: 900;
    src: url("../fonts/montserrat/montserrat-v12-latin-900.ttf"),
      url("../fonts/montserrat/montserrat-v12-latin-900.eot")
        format("embedded-opentype"),
      url("../fonts/montserrat/montserrat-v12-latin-900.svg") format("woff2"),
      url("../fonts/montserrat/montserrat-v12-latin-900.woff") format("woff"),
      url("../fonts/montserrat/montserrat-v12-latin-900.woff2")
        format("truetype");
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    appearance: none !important;
    -moz-appearance: none !important;
    -webkit-appearance: none !important;
    -o-appearance: none !important;
    -ms-appearance: none !important;
    margin: 0;
  }

  input:focus {
    outline: none;
    box-shadow: none !important;
    -moz-box-shadow: none !important;
    -webkit-box-shadow: none !important;
    -o-box-shadow: none !important;
    -ms-box-shadow: none !important;
  }

  h2 {
    line-height: 1.66;
    margin: 0;
    padding: 0;
    font-weight: 900;
    color: #222;
    font-family: "Montserrat";
    font-size: 24px;
    text-transform: uppercase;
    text-align: center;
    margin-bottom: 40px;
  }

  body {
    font-size: 14px;
    line-height: 1.8;
    color: #222;
    font-weight: 400;
    font-family: "Montserrat";
    background-image: url("../assets/images/signup-bg.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    -moz-background-size: cover;
    -webkit-background-size: cover;
    -o-background-size: cover;
    -ms-background-size: cover;
    background-position: center center;
    padding: 115px 0;
  }

  .container {
    width: 660px;
    position: relative;
    margin: 0 auto;
  }

  .signup-content {
    background: #fff;
    border-radius: 10px;
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    padding: 50px 85px;
  }

  .form-group {
    overflow: hidden;
    margin-bottom: 20px;
  }

  .form-input {
    width: 100%;
    border: 1px solid #ebebeb;
    border-radius: 5px;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -o-border-radius: 5px;
    -ms-border-radius: 5px;
    padding: 17px 20px;
    box-sizing: border-box;
    font-size: 14px;
    font-weight: 500;
    color: #222;
    background-repeat: no-repeat;
    padding-left: 50px;
    background-size: 25px;
    background-position-y: center;
    background-position-x: 10px;
  }
  .form-input::-webkit-input-placeholder {
    color: #999;
  }
  .form-input::-moz-placeholder {
    color: #999;
  }
  .form-input:-ms-input-placeholder {
    color: #999;
  }
  .form-input:-moz-placeholder {
    color: #999;
  }
  .form-input::-webkit-input-placeholder {
    font-weight: 500;
  }
  .form-input::-moz-placeholder {
    font-weight: 500;
  }
  .form-input:-ms-input-placeholder {
    font-weight: 500;
  }
  .form-input:-moz-placeholder {
    font-weight: 500;
  }
  .form-input:focus::-webkit-input-placeholder {
    color: #222;
  }
  .form-input:focus::-moz-placeholder {
    color: #222;
  }
  .form-input:focus:-ms-input-placeholder {
    color: #222;
  }
  .form-input:focus:-moz-placeholder {
    color: #222;
  }

  .form-submit {
    width: 100%;
    border-radius: 5px;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -o-border-radius: 5px;
    -ms-border-radius: 5px;
    padding: 17px 20px;
    box-sizing: border-box;
    font-size: 14px;
    font-weight: 700;
    color: #fff;
    text-transform: uppercase;
    border: none;
    background-image: -moz-linear-gradient(to left, #74ebd5, #9face6);
    background-image: -ms-linear-gradient(to left, #74ebd5, #9face6);
    background-image: -o-linear-gradient(to left, #74ebd5, #9face6);
    background-image: -webkit-linear-gradient(to left, #74ebd5, #9face6);
    background-image: linear-gradient(to left, #74ebd5, #9face6);
  }

  .field-icon {
    float: right;
    margin-right: 17px;
    margin-top: -47px;
    position: relative;
    z-index: 2;
    color: #555;
    cursor: pointer;
  }

  @media screen and (max-width: 768px) {
    .container {
      width: calc(100% - 40px);
      max-width: 100%;
    }
  }
  @media screen and (max-width: 480px) {
    .signup-content {
      padding: 50px 25px;
    }
  }
</style>
