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
            <br />
            <div class="firstmenu">
            <div class="w3-container">
                <div
                    on:click={w3_close}
                    class="w3-right closelogo w3-hide-large"
                    title="close menu"
                >
                    <i class="fa fa-remove" style="color:red"/>
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
    <div class="page-content">Hello</div>
</main>

<style>
    .defaultimg {
        height: 80px;
        width: 80px;
        background-image: url("../assets/images/defaultimg.png");
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
        border-radius: 100%;
        position: relative;
        margin-top: -5%;
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
    }
    .showusername {
        margin-left: 30%;
        margin-top: -17%;
        font-size: 17px;
        position: absolute;
    }
    .firstmenu {
        border-bottom: 1px solid rgb(181, 181, 216);
        padding-bottom: 5%;
    }
    @media screen and (min-width: 993px) {
        .page-content {
            margin-left: 300px;
        }
    }
</style>
