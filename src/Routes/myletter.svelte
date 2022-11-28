<script>
    import { Router, Link, Route } from "svelte-routing";
    import axios from "axios";
    import { onMount } from "svelte";
    import {
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        FormGroup,
        Input,
        Spinner,
        Label,
        Tooltip
    } from "sveltestrap";

    let totalLetter = [];
    let userid = "";
    let checking = false;

    function checkLetterExist() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                totalLetter = [];
                totalLetter.push(response.data);
                console.log(totalLetter[0].length);
                if (totalLetter[0].length == 0) {
                    checking = false;
                } else {
                    checking = true;
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function setEditLetter(lid)
    {

    }

    function setDownloadLetter(lid)
    {

    }

    function setShowLetter(lid)
    {

    }

    function generateQRCode(lid)
    {

    }

    function setDeleteLetter(lid)
    {

    }
    onMount(async () => {
        userid = localStorage.getItem(username);
        await checkLetterExist();
    });

    export let username;
    export let url = "";
</script>

<main>
    {#if checking == false}
        <div class="none-added">
            <p>No Letter Added Yet</p>
        </div>
    {:else}
        <div class="letter-details">
            {#each totalLetter[0] as rec, i}
                <div class="tooltip">
                    <Tooltip target="myedit-{i}" placement="bottom"
                        >Edit</Tooltip
                    >
                    <Tooltip target="mydownload-{i}" placement="bottom">
                        Download
                    </Tooltip>
                    <Tooltip target="myview-{i}" placement="bottom"
                        >View</Tooltip
                    >
                    <Tooltip target="myqr-{i}" placement="bottom">
                        Generate QR
                    </Tooltip>
                    <Tooltip target="mydelete-{i}" placement="bottom">
                        Delete
                    </Tooltip>
                </div>
                <div class="card inner-letter">
                    <div>
                        <i
                            class="bi bi-file-earmark-person-fill"
                            style="font-size: 70px; color: #598496;"
                        />
                    </div>
                    <div class="inner-content">
                        <div>
                            <p class="letter-title">{rec.title}</p>
                        </div>
                    </div>
                    <div class="inner-icon">
                        <i
                            class="bi bi-pencil-fill editIcon"
                            id="myedit-{i}"
                            on:click={() => setEditLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                            id="mydownload-{i}"
                            on:click={() => setDownloadLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-eye-fill showIcon"
                            id="myview-{i}"
                            on:click={() => setShowLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-qr-code qrIcon"
                            id="myqr-{i}"
                            on:click={() => generateQRCode(rec.letterid)}
                        />
                        <i
                            class="bi bi-trash-fill deleteIcon"
                            id="mydelete-{i}"
                            on:click={() => setDeleteLetter(rec.letterid)}
                        />
                    </div>
                </div>
            {/each}
            <!-- <Button on:click={temp}>Click</Button> -->
        </div>
    {/if}
    <Router {url}>
        <div class="add-content">
            <Link
                to="/{username}/manage/addletter"
                style="text-decoration: none;"
            >
                <i class="bi bi-plus-circle-fill plusbtn" />
            </Link>
            <p class="add-txt">New Letter</p>
        </div>
    </Router>
</main>

<style>
    .letter-details {
        position: absolute;
        width: 100%;
        margin-top: -4%;
        padding-left: 19%;
        display: inline-block;
    }
    .inner-content {
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
    .inner-letter {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .letter-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .editIcon,
    .downloadIcon,
    .showIcon,
    .qrIcon,
    .deleteIcon {
        cursor: pointer;
        font-size: 15px;
        color: rgb(73, 61, 128);
        border: 0.5px solid rgb(98, 93, 125);
        border-radius: 15px;
        width: 10%;
        text-align: center;
        height: 35px;
        line-height: 35px;
        background-color: aliceblue;
    }
    .editIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .downloadIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .showIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .qrIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .deleteIcon {
        color: red;
    }
    .deleteIcon:hover {
        color: red;
        background-color: rgb(5, 58, 67);
    }
    .none-added {
        position: absolute;
        font-size: 35px;
        font-weight: 400;
        margin-left: 27%;
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
        margin-left: -8%;
        margin-top: -25%;
        font-size: 25px;
        white-space: nowrap;
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
        .letter-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
        }
        .inner-letter {
            width: 70%;
        }
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
        .letter-details {
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
        .letter-details {
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
            margin-left: 37%;
        }
        .letter-details {
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
        .none-added {
            margin-top: 13%;
            margin-left: 33%;
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
            margin-top: 15%;
            margin-left: 30%;
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
        .inner-icon {
            margin-top: -18%;
        }
    }
    @media screen and (max-width: 580px) {
        .add-content {
            margin-left: 80%;
            margin-top: 45%;
        }
        .none-added {
            margin-top: 17%;
            margin-left: 25%;
        }
        .letter-details {
            width: 100%;
            padding-left: 5%;
        }
    }
    @media screen and (max-width: 500px) {
        .add-content {
            margin-left: 78%;
            margin-top: 50%;
        }
        .none-added {
            margin-top: 18%;
            margin-left: 21%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
        }
        .inner-letter {
            width: 95%;
        }
    }
    @media screen and (max-width: 450px) {
        .add-content {
            margin-left: 75%;
            margin-top: 55%;
        }
        .none-added {
            margin-top: 18%;
            margin-left: 19%;
        }
    }
    @media screen and (max-width: 400px) {
        .add-content {
            margin-left: 70%;
            margin-top: 75%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 12%;
        }
        .letter-details {
            margin-top: -15%;
        }
        .inner-icon {
            margin-top: -16%;
            margin-left: 15%;
        }
        .inner-letter {
            margin-top: 5%;
        }
    }
    @media screen and (max-width: 360px) {
        .add-content {
            margin-left: 68%;
            margin-top: 70%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 8%;
        }
    }
    @media screen and (max-width: 340px) {
        .add-content {
            margin-left: 67%;
            margin-top: 75%;
        }
    }
</style>
