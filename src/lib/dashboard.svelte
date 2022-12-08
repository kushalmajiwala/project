<script>
    import { Router, Link, Route } from "svelte-routing";
    import Dashboardhome from "../Routes/dashboardhome.svelte";
    import Mycv from "../Routes/mycv.svelte";
    import Mycover from "../Routes/myletter.svelte";
    import Profile from "../Routes/profile.svelte";
    import Password from "../Routes/password.svelte";
    import Addcv from "../Routes/addcv.svelte";
    import Addletter from "../Routes/addletter.svelte";
    import ManageFormat from "../Routes/manageFormat.svelte";
    import Feedback from "../Routes/feedback.svelte";
    import { onMount } from "svelte";
    import { Tooltip, Button } from "sveltestrap";
    import axios from "axios";
    let showProfile;
    let userid = "";
    let imageurl = "";
    let profile_image_style =
        "height: 70px;width: 70px;border-radius: 100%;background-repeat: no-repeat;background-size: cover;background-position: center center;position: relative; margin-top: 0%;filter: drop-shadow(0 0 0 red);";
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
        //Logic to send on login page after signout
        // let path = window.location.pathname;
        // if (
        //     !path.includes("mycv") &&
        //     !path.includes("mycover") &&
        //     !path.includes("profile") &&
        //     !path.includes("password") &&
        //     !path.includes("addcv") &&
        //     !path.includes("addletter")
        // ) {
        //     window.location.replace(path);
        // }
    }
    export let username;
    export let url = "";
    export let operation;

    onMount(async () => {
        userid = localStorage.getItem(username);
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" +
                userid,
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
                style="z-index:3;width:280px;"
                id="mySidebar"
            >
                <div class="firstmenu">
                    <div
                        on:click={w3_close}
                        class="w3-right closelogo w3-hide-large"
                        title="close menu"
                    >
                        <i
                            class="fa fa-remove"
                            style="color:red; font-size: 25px;"
                        />
                    </div>
                    <div class="w3-container firstmenu1">
                        {#if imageurl === ""}
                            <div class="defaultimage" />
                        {/if}
                        {#if imageurl !== ""}
                            <!-- svelte-ignore a11y-img-redundant-alt -->
                            <img
                                src={imageurl}
                                alt="no-image"
                                style={profile_image_style}
                            />
                        {/if}

                        <Link
                            to="/{username}/manage/profile"
                            style="text-decoration: none;"
                        >
                            <div class="showusername" bind:this={showProfile}>
                                {username}
                            </div>
                            <Tooltip target={showProfile} placement="bottom"
                                >Profile</Tooltip
                            >
                        </Link>
                    </div>
                </div>
                <Link to="/{username}/manage" style="text-decoration: none;">
                    <div class="nav-content">
                        <i class="bi bi-list-ul myicon" />
                        Dashboard
                    </div>
                </Link>
                <Link to="/{username}" style="text-decoration: none;">
                    <div class="nav-content">
                        <i class="bi bi-house-fill myicon" />
                        Home
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
                    to="/{username}/manage/format"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-gear-fill myicon" />
                        Manage Format
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
                <Link
                    to="/{username}/manage/feedback"
                    style="text-decoration: none;"
                >
                    <div class="nav-content">
                        <i class="bi bi-chat-left-text-fill myicon" />
                        Feedback
                    </div>
                </Link>
                <span class="signout-tab">
                    <Link to="/{username}" style="text-decoration: none;">
                        <div class="nav-content" on:click={signout}>
                            <i class="bi bi-box-arrow-right myicon" />
                            Signout
                        </div>
                    </Link>
                </span>
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
                    <div class="signoutbtn">
                        <Link to="/{username}" style="text-decoration: none;">
                            <div on:click={signout}>
                                <Button
                                    color="primary"
                                    style="font-size: 17px; padding: 8px; padding-left: 10px; padding-right: 20px;"
                                >
                                    <i class="bi bi-box-arrow-right myicon" />
                                    Signout
                                </Button>
                            </div>
                        </Link>
                    </div>
                </div>
            </div>
            <br /><br /><br /><br /><br /><br /><br />
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
                {#if operation == "changeFormat"}
                    <Route path="/" component={ManageFormat} />
                {/if}
                {#if operation == "feedback"}
                    <Route path="/" component={Feedback} />
                {/if}
                <Route path="/" component={Dashboardhome} />
            </div>
        </div>
    </Router>
</main>

<style>
    .signout-tab {
        display: none;
    }
    .signoutbtn {
        margin-left: 32%;
        margin-top: -1.5%;
    }
    .defaultimage {
        height: 70px;
        width: 70px;
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
        margin-top: -15px;
        margin-left: 260px;
        /* margin-right: -12%; */
        /* z-index: 3; */
        position: absolute;
    }
    .page-content {
        width: 100%;
        height: 2000px;
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
        padding: 10px;
        box-shadow: 2px 2px 20px 0px grey;
        z-index: 1;
        height: 92px;
        line-height: 92px;
    }
    .mainlogo {
        height: 65px;
        width: 65px;
        background-image: url("../assets/images/cvlogo.jpg");
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
        border-radius: 100%;
        /* margin-top: 0%; */
        box-shadow: 0px 0px 5px 0px rgba(171, 158, 158, 0.8);
        /* margin-right: 3%; */
        /* margin-left: -300px; */
    }
    .set-header {
        display: flex;
        padding-left: 30%;
    }
    .head-text {
        margin-left: 2%;
        font-size: 25px;
        margin-top: -1.3%;
    }
    .showusername {
        /* margin-left: 30%; */
        margin-top: -10px;
        /* background-color: black; */
        height: 60px;
        /* font-size: 17px; */
        /* position: absolute; */
    }
    .firstmenu1 {
        display: flex;
        justify-content: space-evenly;
        height: 92px;
        line-height: 92px;
        padding: 10px;
    }
    .firstmenu {
        /* padding: 3.6%; */
        /* display: flex;
        justify-content: space-between; */
        box-shadow: 2px 2px 5px 0px grey;
        cursor: pointer;

        /* border-radius: 25px; */
        background-color: rgb(228, 245, 245);
    }
    @media screen and (min-width: 1350px) {
        .signoutbtn {
            margin-left: 40%;
            margin-top: -1.5%;
        }
    }
    @media screen and (max-width: 1350px) {
        /* .header {
            padding: 1.17%;
        } */
    }
    @media screen and (max-width: 1230px) {
        /* .header {
            padding: 0.9%;
        } */
        /* .mainlogo {
            height: 80px;
            width: 80px;
        } */
        .set-header {
            /* display: flex; */
            padding-left: 32%;
        }
        .signoutbtn {
            margin-left: 38%;
            margin-top: -1.5%;
        }
    }
    @media screen and (max-width: 1165px) {
        /* .header {
            padding: 1.5%;
        } */
        /* .mainlogo {
            height: 80px;
            width: 80px;
        } */
        .set-header {
            /* display: flex; */
            padding-left: 34%;
        }
        .signoutbtn {
            margin-left: 30%;
            margin-top: -1.5%;
        }
    }
    @media screen and (max-width: 1055px) {
        /* .header {
            padding: 2%;
        } */
        /* .mainlogo {
            height: 80px;
            width: 80px;
        } */
        .set-header {
            display: flex;
            padding-left: 32%;
        }
    }
    @media screen and (min-width: 993px) {
        .page-content {
            margin-left: 280px;
        }
    }
    @media screen and (max-width: 993px) {
        /* .header {
            position: absolute;
            height: 120px;
            display: flex;
            justify-content: center;
            padding: 2%;
            line-height: 120px;
        } */
        /* .head-text {
            margin-top: -2%;
        } */
        .signoutbtn {
           display: none;
        }
        .header {
            position: absolute;
        }
        nav {
            border: none;
        }
        .set-header {
            /* display: flex; */
            padding-left: 39%;
        }
        .signout-tab {
            display: block;
        }
    }
    @media screen and (max-width: 600px) {
        /* .header {
            position: absolute;
            height: 120px;
            padding-left: -7%;
            line-height: 120px;
        } */
        /* .head-text {
            font-size: 27px;
            margin-top: -20px;
        } */
        .set-header {
            /* display: flex; */
            padding-left: 32%;
        }
        .head-text {
            margin-top: -10px;
        }
    }
    @media screen and (max-width: 500px) {
        /* .head-text {
            font-size: 25px;
            margin-top: -2%;
        } */
        /* .header {
            height: 105px;
            padding-left: -7%;
            line-height: 105px;
        } */
    }
    @media screen and (max-width: 460px) {
        .set-header {
            /* display: flex; */
            padding-left: 28%;
        }
        .head-text {
            margin-top: -10px;
        }
    }
    @media screen and (max-width: 400px) {
        /* .head-text {
            font-size: 25px;
        }
        .header {
            height: 110px;
            line-height: 110px;
        } */
        .set-header {
            /* display: flex; */
            padding-left: 23%;
        }
        .head-text {
            margin-top: -10px;
        }
    }
</style>
