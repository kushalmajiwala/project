<main>
    <body>
        <div class="loginBox">
            <!-- svelte-ignore a11y-img-redundant-alt -->
            <img
                class="user"
                src="https://i.ibb.co/yVGxFPR/2.png"
                height="100px"
                width="100px"
                alt="no-image"
            />
            <h3>Sign in here</h3>

            <div class="inputBox">
                <input
                    type="text"
                    name="Username"
                    placeholder="Username"
                    class="usernameicon form-input"
                />
                <input
                    type="password"
                    name="Password"
                    placeholder="Password"
                    class="passwordicon form-input"
                />
            </div>
            <input type="submit" name="" value="Login" />
        </div>
    </body>
</main>

<style>
    .usernameicon {
        background-image: url("../assets/images/usernameIcon.png");
    }
    .passwordicon {
        background-image: url("../assets/images/passwordIcon.png");
    }
    .form-input {
        background-repeat: no-repeat;
        padding-left: 45px;
        background-size: 25px;
        background-position-y: center;
        background-position-x: 10px;
    }
    body {
        background-image: url("../assets/images/signup-bg.jpg");
        height: 100vh;
        font-family: sans-serif;
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center center;
        -moz-background-size: cover;
        -webkit-background-size: cover;
        -o-background-size: cover;
        -ms-background-size: cover;
    }
    @media screen and (max-width: 600px) {
        body {
            background-size: cover;
        }
    }
    .loginBox {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 350px;
        min-height: 200px;
        background: #ffffff;
        border-radius: 10px;
        padding: 40px;
        box-sizing: border-box;
        opacity: 0.8;
    }
    .user {
        margin: 0 auto;
        display: block;
        margin-bottom: 20px;
    }
    h3 {
        margin: 0;
        padding: 0 0 20px;
        color: #0a0b0b;
        text-align: center;
        font-weight: bolder;
    }
    .loginBox input {
        width: 100%;
        margin-bottom: 20px;
    }
    .loginBox input[type="text"],
    .loginBox input[type="password"] {
        border: none;
        border-bottom: 2px solid #464444;
        outline: none;
        height: 40px;
        color: #fff;
        font-size: 16px;
        box-sizing: border-box;
    }
    .loginBox input[type="text"]:hover,
    .loginBox input[type="password"]:hover {
        color: #42f3fa;
        border: 1px solid #42f3fa;
        box-shadow: 0 0 5px rgba(0, 255, 0, 0.3), 0 0 10px rgba(0, 255, 0, 0.2),
            0 0 15px rgba(0, 255, 0, 0.1), 0 2px 0 black;
    }
    .loginBox input[type="text"]:focus,
    .loginBox input[type="password"]:focus {
        border-bottom: 2px solid #42f3fa;
    }
    .inputBox {
        position: relative;
    }
    .loginBox input[type="submit"] {
        border: none;
        outline: none;
        height: 40px;
        font-size: 16px;
        background: #59238f;
        color: #fff;
        border-radius: 20px;
        cursor: pointer;
    }
</style>
