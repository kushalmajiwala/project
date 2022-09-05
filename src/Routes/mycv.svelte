<script>
    import { Router, Link, Route } from "svelte-routing";
    import { onMount } from "svelte";
    import axios from "axios";

    let totalCV = [];
    let checking = false;

    export let username;
    export let url = "";

    let userid = localStorage.getItem(username);

    function checkCvExist()
    {
        
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/userid/" + userid,
        };

        axios
            .request(options)
            .then(function (response) {
                totalCV.push(response.data);
                console.log(totalCV[0].length);
                checking = true;
            })
            .catch(function (error) {
                console.error(error);
            });
    }

    onMount(async () => {
       await checkCvExist();
    });
</script>

<main>
    {#if checking == false}
    <div class="none-added">
        <p>No CV Added Yet</p>
    </div>
    {:else }
    <div class="cv-details">
        {#each totalCV[0] as rec}
            <!-- <div class="card inner-cv">
                hello
            </div> -->
            <div class="card inner-cv">
                <div>
                    <i class="bi bi-file-earmark-person-fill" style="font-size: 70px; color: #598496;"></i>
                </div>
                <div class="inner-content">
                    <div>
                        <p class="cv-title">{rec.cvtitle}</p>
                    </div>
                </div>
                <div class="inner-icon">
                    <i class="bi bi-pencil-fill editIcon"></i>
                    <i class="bi bi-file-earmark-arrow-down-fill downloadIcon"></i>
                    <i class="bi bi-eye-fill showIcon"></i>
                    <i class="bi bi-trash-fill deleteIcon"></i>
                </div>
            </div>
        {/each}
    </div>
    {/if}
    <!-- <button on:click={checkCvExist}>Click</button> -->
    <Router {url}>
        <div class="add-content">
            <Link to="/{username}/manage/addcv" style="text-decoration: none;">
                <i class="bi bi-plus-circle-fill plusbtn" />
            </Link>
            <p class="add-txt">New CV</p>
        </div>
    </Router>
</main>

<style>
    .editIcon, .downloadIcon, .showIcon, .deleteIcon {
        cursor: pointer;
        font-size: 25px;
    }
    .deleteIcon {
        color: red;
    }
    .cv-details {
        position: absolute;
        width: 100%;
        margin-top: -4%;
        padding-left: 19%;
        display: inline-block;
        /* justify-content: center; */
        /* margin-left: 14%; */
    }
    .inner-cv {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .inner-content
    {
        display: flex;
        justify-content: center;
        width: 100%;
        padding-top: -5%;
        position: absolute;
    }
    .inner-icon {
        display: flex;
        justify-content: space-evenly;
        margin-top: -10%;
        margin-left: 8%;
    }
    .cv-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color:rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .none-added {
        position: absolute;
        font-size: 35px;
        font-weight: 400;
        margin-left: 28%;
        margin-top: 10%;
        text-shadow: 0px 5px 10px rgba(61, 51, 51, 0.25);
    }
    .add-content {
        margin-left: 65%;
        margin-top: 20%;
        position: fixed;
    }
    .add-txt {
        text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        color: black;
        margin-left: 0%;
        margin-top: -25%;
        font-size: 25px;
    }
    .plusbtn {
        color: rgb(100, 86, 167);
        font-size: 90px;
        cursor: pointer;
    }
    .plusbtn:hover {
        color: rgb(85, 69, 155);
    }
    @media screen and (min-width: 1350px) {
        .add-content {
            margin-left: 71%;
            margin-top: 26%;
        }
    }
    @media screen and (max-width: 1230px) {
        .add-content {
            margin-left: 70%;
            margin-top: 30%;
        }
        .none-added {
            margin-left: 40%;
            margin-top: 18%;
        }
        .cv-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
        }
        .inner-cv {
            width: 70%;
        }
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
        .cv-details {
            width: 100%;
            margin-left: 0%;
            padding-left: 25%;
        }
    }
    @media screen and (max-width: 1100px) {
        .add-content {
            margin-left: 70%;
            margin-top: 36%;
        }
        .cv-details {
            padding-left: 30%;
        }
    }
    @media screen and (max-width: 1070px) {
        .add-content {
            margin-left: 67%;
            margin-top: 38%;
        }
        .none-added {
            margin-top: 23%;
        }
    }
    @media screen and (max-width: 995px) {
        .add-content {
            margin-left: 85%;
            margin-top: 25%;
        }
        .none-added {
            margin-top: 10%;
        }
        .cv-details {
            padding-left: 25%;
        }
    }
    @media screen and (max-width: 900px) {
        .add-content {
            margin-left: 85%;
            margin-top: 27%;
        }
        .none-added {
            margin-top: 12%;
            margin-left: 35%;
        }
    }
    @media screen and (max-width: 820px) {
        .add-content {
            margin-left: 85%;
            margin-top: 30%;
        }
        .inner-icon {
          margin-top: -12%;
        }
    }
    @media screen and (max-width: 740px) {
        .add-content {
            margin-left: 85%;
            margin-top: 35%;
        }
        .none-added {
            margin-top: 14%;
            margin-left: 32%;
        }
        .inner-icon {
          margin-top: -13%;
          margin-left: 15%;
        }
    }
    @media screen and (max-width: 640px) {
        .add-content {
            margin-left: 83%;
            margin-top: 40%;
        }
        .none-added {
            margin-top: 15%;
            margin-left: 30%;
        }
    }
    @media screen and (max-width: 580px) {
        .add-content {
            margin-left: 80%;
            margin-top: 45%;
        }
    }
    @media screen and (max-width: 500px) {
        .add-content {
            margin-left: 78%;
            margin-top: 50%;
        }
        .none-added {
            margin-top: 16%;
            margin-left: 26%;
        }
        .cv-details {
           width: 100%;
           padding-left: 5%;
        }
        .inner-cv {
            width: 95%;
        }
        .inner-icon {
          margin-top: -11%;
          margin-left: 10%;
        }
    }
    @media screen and (max-width: 450px) {
        .add-content {
            margin-left: 75%;
            margin-top: 55%;
        }
        .none-added {
            margin-top: 17%;
            margin-left: 23%;
        }
    }
    @media screen and (max-width: 400px) {
        .add-content {
            margin-left: 70%;
            margin-top: 75%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 18%;
        }
        .inner-icon {
          margin-top: -15%;
          margin-left: 15%;
        }
        .cv-details {
            margin-top: -15%;
        }
        .inner-cv {
            margin-top: 5%;
        }
    }
    @media screen and (max-width: 360px) {
        .add-content {
            margin-left: 70%;
            margin-top: 70%;
        }
        .none-added {
            margin-top: 19%;
            margin-left: 16%;
        }
    }
    @media screen and (max-width: 340px) {
        .add-content {
            margin-left: 67%;
            margin-top: 75%;
        }
    }
</style>
