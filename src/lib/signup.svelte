<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import { Button, Modal, ModalBody, ModalFooter } from "sveltestrap";
  let togglebtn = "bi bi-eye";
  let passtype = "password";
  let username = "";
  let email = "";
  let password = "";
  let rpassword = "";
  let open1 = false;
  let open2 = false;
  let open3 = false;
  let open4 = false;
  let open5 = false;
  let open6 = false;
  let open7 = false;
  let btnname = "SIGN UP";
  let borderUsername = "border: 2px solid #ebebeb";
  let borderEmail = "border: 2px solid #ebebeb";
  let borderPassword = "border: 2px solid #ebebeb";
  let borderRpassword = "border: 2px solid #ebebeb";
  const toggle1 = () => (open1 = !open1);
  const toggle2 = () => (open2 = !open2);
  const toggle3 = () => (open3 = !open3);
  const toggle4 = () => (open4 = !open4);
  const toggle5 = () => (open5 = !open5);
  const toggle6 = () => (open6 = !open6);
  const toggle7 = () => (open7 = !open7);
  const changeName = () => (btnname = "SIGN UP");

  function toggle() {
    if (togglebtn === "bi bi-eye-slash") {
      togglebtn = "bi bi-eye";
      passtype = "password";
    } else {
      togglebtn = "bi bi-eye-slash";
      passtype = "text";
    }
  }
  function signupAuthentication() {
    btnname = "PLEASE WAIT..."
    if (username !== "" && email !== "" && password !== "" && rpassword !== "") {
      borderUsername = "border: 2px solid #ebebeb";
      borderEmail = "border: 2px solid #ebebeb";
      borderPassword = "border: 2px solid #ebebeb";
      borderRpassword = "border: 2px solid #ebebeb";
    } 
    if (username == "" || email == "" || password == "" || rpassword == "") 
    {
      toggle1();
      if(username == "") borderUsername = "border: 2px solid red"; else borderUsername = "border: 2px solid #ebebeb";
      if(email == "") borderEmail = "border: 2px solid red"; else borderEmail = "border: 2px solid #ebebeb";
      if(password == "") borderPassword = "border: 2px solid red"; else borderPassword = "border: 2px solid #ebebeb";
      if(rpassword == "") borderRpassword = "border: 2px solid red"; else borderRpassword = "border: 2px solid #ebebeb";
    } 
    else if(!email.includes('@') || !email.includes('.')) 
    {
      toggle6();
    }
    else if(password.length < 8)
    {
      toggle7();
    } 
    else 
    {
      const options1 = {
        method: "GET",
        url:
          "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/signup/username/" +
          username,
      };

      axios
        .request(options1)
        .then(function () {
          toggle2();
        })
        .catch(function () {
          const options2 = {
            method: "GET",
            url:
              "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/signup/email/" +
              email,
          };

          axios
            .request(options2)
            .then(function () {
              toggle3();
            })
            .catch(function () {
              if (password !== rpassword) {
                toggle4();
              } else {
                let rec = {
                  username: username,
                  email: email,
                  password: password,
                };
                axios
                  .post(
                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/signup/",
                    rec
                  )
                  .then(function () {
                    toggle5();
                  });
              }
            });
        });
    }
  }
  onMount(async () => {});
</script>

<main>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <!-- <meta http-equiv="X-UA-Compatible" content="ie=edge" /> -->
      <title>Sign Up</title>
    </head>
    <body>
      <div class="logocontainer">
        <div class="mainlogo" />
      </div>
      <div class="main">
        <section class="signup">
          <div class="container">
            <div class="signup-content">
              <h2 class="form-title">Create account</h2>
              <div class="form-group">
                <input
                  autocomplete="false"
                  name="hidden"
                  type="text"
                  class="usernameicon form-input"
                  id="name"
                  placeholder="Username"
                  bind:value={username}
                  style={borderUsername}
                />
              </div>
              <div class="form-group">
                <input
                  type="email"
                  class="emailicon form-input"
                  autocomplete="false"
                  name="hidden"
                  id="email"
                  placeholder="Email"
                  bind:value={email}
                  style={borderEmail}
                />
              </div>
              <div class="form-group">
                {#if passtype === "password"}
                  <input
                    type="password"
                    class="passwordicon form-input"
                    placeholder="Password"
                    bind:value={password}
                    style={borderPassword}
                  />
                {:else}
                  <input
                    type="text"
                    class="passwordicon form-input"
                    placeholder="Password"
                    bind:value={password}
                    style={borderPassword}
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
                  style={borderRpassword}
                />
              </div>
              <div class="form-group">
                <input
                  type="submit"
                  name="submit"
                  id="submit"
                  class="form-submit"
                  value={btnname}
                  on:click={signupAuthentication}
                />
              </div>
            </div>
          </div>
        </section>
      </div>
    </body>
  </html>
  <div class="modals">
    <!-- empty-modal -->
    <Modal isOpen={open1}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Fields Cannot be empty
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle1} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- username Checking Modal -->
    <Modal isOpen={open2}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Username Already Exists...Please Select other username
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle2} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- email Checking Modal -->
    <Modal isOpen={open3}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Email Already Exists...Please Select other email
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle3} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- Password Checking Modal -->
    <Modal isOpen={open4}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Password and Confirm Password does not match
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle4} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- Email Validation  -->
    <Modal isOpen={open6}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Invalid Email
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle6} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
     <!-- Password Validation  -->
     <Modal isOpen={open7}>
      <ModalFooter>
        <div class="empty-symbol-container">
          <i class="bi bi-x-circle empty-symbol" />
        </div>
        <div class="empty-symbol-container">
          <p class="empty-txt">
            Password must be 8 characters long
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle7} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
    <!-- Successfull Registration -->
    <Modal isOpen={open5}>
      <ModalFooter>
        <div class="create-symbol-container">
          <i class="bi bi-check-lg create-symbol" />
        </div>
        <div class="create-symbol-container">
          <p class="create-txt">
            You are Registered Successfully
          </p>
        </div>
        <Button color="danger" class="float-right" on:click={toggle5} on:click={changeName}
          >Cancel</Button
        >
      </ModalFooter>
    </Modal>
  </div>
</main>

<style>
  .create-txt {
        font-size: 20px;
    }
    .create-symbol-container,
    .empty-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .create-symbol {
        font-size: 70px;
        color: rgb(39, 138, 63);
    }
    .empty-symbol {
        font-size: 70px;
        color: rgb(237, 59, 59);
    }
  .logocontainer {
    display: flex;
    justify-content: center;
  }
  .mainlogo {
    height: 120px;
    width: 120px;
    background-image: url("../assets/images/cvlogo.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center center;
    border-radius: 100%;
    position: absolute;
    margin-top: -5%;
    z-index: 1;
    filter: drop-shadow(0 0 0 red); 
  }
  .form-title {
    white-space: nowrap;
    margin-top: 5%;
  }
  .usernameicon {
    background-image: url("../assets/images/usernameIcon.png");
  }
  .emailicon {
    background-image: url("../assets/images/mailIcon.png");
  }
  .passwordicon {
    background-image: url("../assets/images/passwordIcon.png");
  }
  .confirmpasswordicon {
    background-image: url("../assets/images/confirmPasswordIcon.png");
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
    width: 535px;
    position: relative;
    margin: 0 auto;
    display: flex;
    align-items: center;
    display: flex;
    justify-content: center;
  }

  .signup-content {
    background: #ffffff;
    border-radius: 10px;
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    padding: 50px 100px;
    padding-bottom: 30px;
  }

  .form-group {
    overflow: hidden;
    margin-bottom: 20px;
  }

  .form-input {
    width: 100%;
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
  .form-input{
    width: 100%;
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
      /* max-width: 100%; */
    }
  }
  @media screen and (max-width: 480px) {
    .signup-content {
      padding: 40px 40px;
    }
  }
  @media screen and (max-width: 400px) {
    .signup-content {
      padding: 10px 55px;
    }
  }
  @media screen and (max-width: 300px) {
    .signup-content {
      padding: 10px 30px;
    }
    .container {
      width: 270px;
      margin-right: 10px;
    }
  }
  @media screen and (max-width: 1250px) {
    .mainlogo {
      margin-top: -5%;
    }
  }
  @media screen and (max-width: 1150px) {
    .mainlogo {
      margin-top: -6%;
    }
  }
  @media screen and (max-width: 1050px) {
    .mainlogo {
      margin-top: -6.5%;
    }
  }
  @media screen and (max-width: 900px) {
    .mainlogo {
      margin-top: -7%;
    }
  }
  @media screen and (max-width: 820px) {
    .mainlogo {
      margin-top: -7.5%;
    }
  }
  @media screen and (max-width: 770px) {
    .mainlogo {
      margin-top: -8%;
    }
  }
  @media screen and (max-width: 720px) {
    .mainlogo {
      margin-top: -8.5%;
    }
  }
  @media screen and (max-width: 690px) {
    .mainlogo {
      margin-top: -9%;
    }
  }
  @media screen and (max-width: 620px) {
    .mainlogo {
      margin-top: -9.5%;
    }
  }
  @media screen and (max-width: 600px) {
    .mainlogo {
      margin-top: -10%;
    }
  }
  @media screen and (max-width: 550px) {
    .mainlogo {
      margin-top: -11%;
    }
  }
  @media screen and (max-width: 500px) {
    .mainlogo {
      margin-top: -12%;
    }
  }
  @media screen and (max-width: 480px) {
    .mainlogo {
      margin-top: -13%;
    }
  }
  @media screen and (max-width: 440px) {
    .mainlogo {
      margin-top: -14%;
      height: 110px;
      width: 110px;
    }
  }
  @media screen and (max-width: 400px) {
    .mainlogo {
      margin-top: -15%;
    }
    .form-title {
      margin-top: 20%;
    }
  }
  @media screen and (max-width: 320px) {
    .mainlogo {
      margin-top: -17%;
    }
  }
</style>
