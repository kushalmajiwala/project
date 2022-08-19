<script>
    // Script to open and close sidebar
    function w3_open() {
        document.getElementById("mySidebar").style.display = "block";
        document.getElementById("myOverlay").style.display = "block";
    }

    function w3_close() {
        document.getElementById("mySidebar").style.display = "none";
        document.getElementById("myOverlay").style.display = "none";
    }
    export let username;
</script>

<main>
    <div class="w3-light-grey w3-content" style="max-width:1600px">
        <!-- Sidebar/menu -->
        <nav
            class="w3-sidebar w3-collapse w3-white w3-animate-left"
            style="z-index:3;width:300px;"
            id="mySidebar"
        >
            <div class="firstmenu">
                <div class="w3-container">
                    <div
                        on:click={w3_close}
                        class="w3-right closelogo w3-hide-large"
                        title="close menu"
                    >
                        <i class="fa fa-remove" style="color:red" />
                    </div>
                    <div class="defaultimg" />
                    <div class="showusername">{username}</div>
                </div>
            </div>
        </nav>
        <div
            class="w3-overlay w3-hide-large w3-animate-opacity"
            on:click={w3_close}
            style="cursor:pointer"
            title="close side menu"
            id="myOverlay"
        />
        <div class="w3-main" style="margin-left:300px">
            <header id="portfolio">
                <span
                    class="w3-button w3-hide-large w3-xxlarge w3-hover-text-grey"
                    on:click={w3_open}><i class="fa fa-bars" /></span
                >
            </header>
        </div>
    </div>
    <div class="page-content">
        <div class="header">
            <div class="set-header w3-container">
                <div class="mainlogo" />
                <div class="head-text"><p>My CV Creator</p></div>
            </div>
        </div>
    </div>
</main>

<style>
    nav {
        border-right: 2px solid #646cbc;
    }
    .defaultimg {
        height: 80px;
        width: 80px;
        background-image: url("../assets/images/defaultimg.png");
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
        border-radius: 100%;
        position: relative;
        margin-top: 0%;
        filter: drop-shadow(0 0 0 red);
    }
    .closelogo {
        font-size: 30px;
        margin-top: -10%;
        margin-right: -5%;
    }
    .page-content {
        width: 100%;
        height: 1000px;
        background-color: #dde0f2;
        position: relative;
    }
    .header {
        background-color: white;
        width: 100%;
        border-bottom: 2px solid #646cbc;
        position: fixed;
        padding: 1.17%;
    }
    .mainlogo {
        height: 80px;
        width: 80px;
        background-image: url("../assets/images/cvlogo.jpg");
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
        border-radius: 100%;
        margin-top: 0%;
        box-shadow: 0px 0px 7px 3px rgba(153, 153, 153, 0.8);
        margin-right: 3%;
    }
    .set-header {
        display: flex;
        justify-content: center;
        width: 100%;
        margin-left: -12%;
    }
    .head-text {
        font-size: 35px;
        margin-top: 1%;
    }
    .showusername {
        margin-left: 30%;
        margin-top: -17%;
        font-size: 17px;
        position: absolute;
    }
    .firstmenu {
        border-bottom: 2px solid #646cbc;
        padding: 5%;
    }
    @media screen and (max-width: 1230px) {
        .header {
            padding: 0.9%;
        }
    }
    @media screen and (max-width: 1165px) {
        .header {
            padding: 1%;
        }
    }
    @media screen and (max-width: 1055px) {
        .header {
            padding: 1.1%;
        }
    }
    @media screen and (min-width: 993px) {
        .page-content {
            margin-left: 300px;
        }
    }
    @media screen and (max-width: 993px) {
        .firstmenu {
            border-right: none;
        }
        .header {
            position: absolute;
            height: 12%;
            display: flex;
            justify-content: center;
            padding: 2%;
        }
        nav {
            border: none;
        }
    }
    @media screen and (max-width: 740px) {
        .firstmenu {
            border-right: none;
        }
        .header {
            position: absolute;
            height: 10%;
            padding-left: -7%;
        }
        .head-text {
            font-size: 27px;
            margin-top: 2%;
        }
    }
    @media screen and (max-width: 500px) {
        .head-text {
            font-size: 25px;
            margin-top: 4%;
        }
    }
</style>
