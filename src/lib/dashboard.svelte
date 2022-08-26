<script>
    import { Router, Link, Route } from "svelte-routing";
    import Dashboardhome from "../Routes/dashboardhome.svelte";
    import Mycv from "../Routes/mycv.svelte";
    import Mycover from "../Routes/myletter.svelte";
    import Profile from "../Routes/profile.svelte";
    import Password from "../Routes/password.svelte";
    import Addcv from "../Routes/addcv.svelte";
    import Addletter from "../Routes/addletter.svelte";
    import { onMount } from "svelte";
    import { Tooltip } from "sveltestrap";
    import axios from "axios";
    let showProfile;
    let userid = "";
    let imageurl = "";
    let profile_image_style = "height: 80px;width: 80px;border-radius: 100%;background-repeat: no-repeat;background-size: cover;background-position: center center;position: relative; margin-top: 0%;filter: drop-shadow(0 0 0 red);";
    // Script to open and close sidebar
    function w3_open() {
        document.getElementById("mySidebar").style.display = "block";
        document.getElementById("myOverlay").style.display = "block";
    }

    function w3_close() {
        document.getElementById("mySidebar").style.display = "none";
        document.getElementById("myOverlay").style.display = "none";
    }
    function signout() {
        localStorage.removeItem(username);
        let path = window.location.pathname;
        if (
            !path.includes("mycv") &&
            !path.includes("mycover") &&
            !path.includes("profile") &&
            !path.includes("password") &&
            !path.includes("addcv") &&
            !path.includes("addletter")
        ) {
            window.location.replace(path);
        }
    }
    export let username;
    export let url = "";
    export let operation;

    onMount(async () => {
        userid = localStorage.getItem(username);
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" + userid,
        };
        axios
            .request(options)
            .then(function (response) {
                imageurl = response.data.image;
            })
            .catch(function (error) {
                console.error(error);
            });
    });
</script>

<main>
    <Router {url}>
        <div class="w3-light-grey w3-content" style="max-width:1600px">
            <!-- Sidebar/menu -->
            <nav
                class="w3-sidebar w3-collapse w3-white w3-animate-left"
                style="z-index:3;width:300px;"
                id="mySidebar"
            >
                <Tooltip target={showProfile}>Profile</Tooltip>

                <div class="firstmenu">
                    <div class="w3-container">
                        <div
                            on:click={w3_close}
                            class="w3-right closelogo w3-hide-large"
                            title="close menu"
                        >
                            <i class="fa fa-remove" style="color:red" />
                        </div>
                        {#if imageurl === ""}
                            <div class="defaultimage"/>
                        {/if}
                        {#if imageurl !== ""}
                            <!-- svelte-ignore a11y-img-redundant-alt -->
                            <img src={imageurl} alt="no-image" style={profile_image_style}>
                        {/if}

                        <Link
                            to="/{username}/manage/profile"
                            style="text-decoration: none;"
                        >
                            <div class="showusername" bind:this={showProfile}>
                                {username}
                            </div>
                        </Link>
                    </div>
                </div>
                <Link to="/{username}/manage" style="text-decoration: none;">
                    <div class="nav-content">
                        <i class="bi bi-list-ul myicon" />
                        Dashboard
                    </div>
                </Link>
                <Link
                    to="/{username}/manage/mycv"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-file-earmark-fill myicon" />
                        My CV
                    </div>
                </Link>
                <Link
                    to="/{username}/manage/mycover"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-file-earmark-fill myicon" />
                        My Cover Letter
                    </div>
                </Link>
                <Link
                    to="/{username}/manage/profile"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-person-fill myicon" />
                        Profile
                    </div>
                </Link>
                <Link
                    to="/{username}/manage/password"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-lock-fill myicon" />
                        Password
                    </div>
                </Link>
                <Link to="/{username}/manage" style="text-decoration: none;">
                    <div class="nav-content" on:click={signout}>
                        <i class="bi bi-box-arrow-right myicon" />
                        Signout
                    </div>
                </Link>
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
                        class="w3-button w3-hide-large w3-large w3-hover-text-grey"
                        on:click={w3_open}><i class="fa fa-bars" /></span
                    >
                </header>
            </div>
        </div>
        <div class="page-content">
            <div class="header">
                <div class="set-header w3-container">
                    <div class="mainlogo" />
                    <div class="head-text"><p>CV Creator</p></div>
                </div>
            </div>
            <br /><br /><br /><br /><br /><br /><br /><br />
            <div class="route-content">
                {#if operation == "mycv"}
                    <Route path="/" component={Mycv} />
                {/if}
                {#if operation == "mycover"}
                    <Route path="/" component={Mycover} />
                {/if}
                {#if operation == "profile"}
                    <Route path="/" component={Profile} />
                {/if}
                {#if operation == "password"}
                    <Route path="/" component={Password} />
                {/if}
                {#if operation == "addcv"}
                    <Route path="/" component={Addcv} />
                {/if}
                {#if operation == "addletter"}
                    <Route path="/" component={Addletter} />
                {/if}
                <Route path="/" component={Dashboardhome} />
            </div>
        </div>
    </Router>
</main>

<style>
    .defaultimage {
        height: 80px;
        width: 80px;
        border-radius: 100%;
        background-image: url("../assets/images/defaultimg.png");
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
        position: relative;
        margin-top: 0%;
        filter: drop-shadow(0 0 0 red);
    }
    .nav-content {
        display: flex;
        justify-content: left;
        padding: 15px;
        font-size: 18px;
        /* border-bottom: 1px solid #d7ddea; */
        cursor: pointer;
        color: #3a4394;
        box-shadow: 1px 1px 1px 0px #bfcade;
    }
    .nav-content:hover {
        background-color: #646cbc;
        color: white;
    }
    .myicon {
        margin-right: 7%;
        margin-left: 2%;
    }
    nav {
        box-shadow: 2px 2px 15px 0px grey;
    }
    .closelogo {
        font-size: 30px;
        margin-top: -10%;
        margin-right: -12%;
    }
    .page-content {
        width: 100%;
        height: 1200px;
        background-color: #dde0f2;
        position: relative;
    }
    .route-content {
        position: relative;
        z-index: 0;
    }
    .header {
        background-color: white;
        width: 100%;
        position: fixed;
        padding: 1.17%;
        box-shadow: 2px 2px 20px 0px grey;
        z-index: 1;
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
        box-shadow: 0px 0px 10px 0px rgba(153, 153, 153, 0.8);
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
        padding: 5%;
        box-shadow: 2px 2px 5px 0px grey;
        cursor: pointer;
        /* border-radius: 25px; */
        background-color: rgb(228, 245, 245);
    }
    @media screen and (min-width: 1350px) {
        .header {
            padding: 0.9%;
        }
    }
    @media screen and (max-width: 1350px) {
        .header {
            padding: 1.17%;
        }
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
