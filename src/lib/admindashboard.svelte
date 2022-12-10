<script>
    import axios from "axios";
    import {
        Badge,
        Form,
        FormGroup,
        Input,
        Label,
        Button,
        Modal,
        ModalFooter,
        ModalHeader,
    } from "sveltestrap";
    import { onMount } from "svelte";
    let total_user = "";
    let total_cv = "";
    let total_letter = "";

    let dark = true;
    let light = false;

    onMount(async () => {
        //Total User
        const options1 = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/login/",
        };

        axios
            .request(options1)
            .then(function (response) {
                total_user = response.data.length;
            })
            .catch(function (error) {
                console.error(error);
            });
        //Total CV
        const options2 = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/",
        };

        axios
            .request(options2)
            .then(function (response) {
                total_cv = response.data.length;
            })
            .catch(function (error) {
                console.error(error);
            });

        //Total Letters
        const options3 = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/",
        };

        axios
            .request(options3)
            .then(function (response) {
                total_letter = response.data.length;
            })
            .catch(function (error) {
                console.error(error);
            });
    });
    function togglebtn()
    {
        if(dark)
        {
            document.getElementById('main').style.backgroundColor = "aliceblue";
            dark = false;
        }
        else
        {
            dark = true;
        }
        if(light)
        {
            document.getElementById('main').style.backgroundColor = "rgb(10, 9, 10)";
            light = false;
        }
        else
        {
            light = true;
        }
    }

    export let username;
</script>

<main class="main" id="main">
    <nav class="navbar navbar-dark bg-dark" style="color: white;">
        <div class="togglebtn" on:click={togglebtn}>
            {#if dark}
            <p><i class="bi bi-brightness-high"></i><span style="margin-left: 7%;">Light Mode</span></p>
            {/if}
            {#if light}
            <p><i class="bi bi-moon"></i><span style="margin-left: 7%;">Dark Mode</span></p>
            {/if}
        </div>
    </nav>

    <div class="main-container">
        <!-- Total User -->
        <div class="total-user">
            <div class="inner-user-total">
                <p>Total-User</p>
                <p><i class="bi bi-people-fill" /></p>
            </div>
            <div class="inner-user-total1">
                <p>{total_user}</p>
                <p />
            </div>
            <div class="inner-user-total2">
                <p>Increased By 2%</p>
            </div>
        </div>
        <!-- Total CV -->
        <div class="total-cv">
            <div class="inner-user-total">
                <p>Total-CV</p>
                <p><i class="bi bi-file-earmark-person-fill" /></p>
            </div>
            <div class="inner-user-total1">
                <p>{total_cv}</p>
                <p />
            </div>
            <div class="inner-user-total2">
                <p>Increased By 3%</p>
            </div>
        </div>
        <!-- Total Letter -->
        <div class="total-letter">
            <div class="inner-user-total">
                <p>Total-Letter</p>
                <p><i class="bi bi-envelope-paper-fill" /></p>
            </div>
            <div class="inner-user-total1">
                <p>{total_letter}</p>
                <p />
            </div>
            <div class="inner-user-total2">
                <p>Increased By 1%</p>
            </div>
        </div>
    </div>
    <div class="round-graph" />
</main>

<style>
    .togglebtn {
        background-color:rgb(31, 30, 31);
        color: white;
        height: 50px;
        width: 150px;
        border-radius: 15px;
        text-align: center;
        line-height: 50px;
        cursor: pointer;
        margin-left: 90%;
    }
    .togglebtn:hover {
        background-color:rgb(44, 39, 44);
    }
    .inner-user-total {
        display: flex;
        justify-content: space-around;
        padding-top: 8%;
    }
    .inner-user-total1 {
        display: flex;
        justify-content: space-around;
        padding-top: 2%;
    }
    .inner-user-total2 {
        display: flex;
        justify-content: space-around;
        padding-top: 2%;
        font-size: 15px;
    }
    .main {
        background-color: rgb(10, 9, 10);
        height: 800px;
    }
    .main-container {
        display: flex;
        justify-content: space-around;
        padding-top: 5%;
        width: 100%;
        height: 100%;
        padding-bottom: 20%;
    }
    .total-user {
        /* background-color: rgb(253, 92, 18); */
        background-image: linear-gradient(
            to right,
            rgb(250, 170, 170),
            rgb(253, 92, 18)
        );
    }
    .total-cv {
        /* background-color: rgb(15, 129, 251); */
        background-image: linear-gradient(
            to right,
            rgb(170, 170, 251),
            rgb(15, 129, 251)
        );
    }
    .total-letter {
        /* background-color: rgb(37, 209, 34); */
        background-image: linear-gradient(
            to right,
            rgb(170, 209, 170),
            rgb(37, 209, 34)
        );
    }
    .total-user,
    .total-cv,
    .total-letter {
        height: 200px;
        width: 350px;
        border-radius: 25px;
        color: white;
        font-size: 25px;
    }
    @media screen and (max-width: 1100px) {
        .main-container {
            display: block;
            padding-left: 35%;
        }
        .total-user,
        .total-cv,
        .total-letter {
            margin-top: 2%;
        }
        .togglebtn {
            width: 150px;
            margin-left: 80%;
        }
    }
    @media screen and (max-width: 950px) {
        .main-container {
            display: block;
            padding-left: 30%;
        }
    }
    @media screen and (max-width: 900px) {
        .main-container {
            display: block;
            padding-left: 27%;
        }
    }
    @media screen and (max-width: 750px) {
        .main-container {
            display: block;
            padding-left: 25%;
        }
    }
    @media screen and (max-width: 650px) {
        .main-container {
            display: block;
            padding-left: 20%;
        }
    }
    @media screen and (max-width: 600px) {
       .togglebtn {
        margin-left: 70%;
       }
    }
    @media screen and (max-width: 535px) {
        .main-container {
            display: block;
            padding-left: 15%;
        }
    }
    @media screen and (max-width: 460px) {
        .main-container {
            display: block;
            padding-left: 10%;
        }
    }
    @media screen and (max-width: 425px) {
        .main-container {
            display: block;
            padding-left: 5%;
        }
        .togglebtn {
            margin-left: 60%;
        }
    }
    @media screen and (max-width: 400px) {
        .main-container {
            display: block;
            padding-left: 3%;
        }
    }
    @media screen and (max-width: 370px) {
        .main-container {
            display: block;
            padding-left: 1.5%;
        }
    }
</style>
