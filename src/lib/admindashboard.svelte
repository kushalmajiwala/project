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
        Tooltip,
    } from "sveltestrap";
    import { onMount } from "svelte";
    import Plotly from "plotly.js-dist-min";

    let no_feedback_style = "color:aliceblue;"

    let total_user = 0;
    let total_cv = 0;
    let total_letter = 0;

    let totalFeedback = [];
    let checking = false;

    let dark = true;
    let light = false;
    let delete_fid = "";

    let open1 = false;
    let open2 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);

    function getData() {
        //Total User
        const options1 = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/login/",
        };

        axios
            .request(options1)
            .then(function (response) {
                total_user = response.data.length;

                //Total CV
                const options2 = {
                    method: "GET",
                    url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/",
                };

                axios
                    .request(options2)
                    .then(function (response) {
                        total_cv = response.data.length;

                        //Total Letters
                        const options3 = {
                            method: "GET",
                            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/",
                        };

                        axios
                            .request(options3)
                            .then(function (response) {
                                total_letter = response.data.length;

                                //For Bar Chart
                                var xArray = ["User", "CV", "Letter"];
                                var yArray = [
                                    total_user,
                                    total_cv,
                                    total_letter,
                                ];

                                var data = [
                                    {
                                        x: xArray,
                                        y: yArray,
                                        type: "bar",
                                    },
                                ];

                                var layout = {
                                    title: "Bar Chart of Application",
                                };

                                Plotly.newPlot("barPlot", data, layout);

                                //For Pie Chart
                                var xArray1 = ["User", "CV", "Letter"];
                                var yArray1 = [
                                    total_user,
                                    total_cv,
                                    total_letter,
                                ];

                                var layout1 = {
                                    title: "Pie Chart of Application",
                                };

                                var data1 = [
                                    {
                                        labels: xArray1,
                                        values: yArray1,
                                        type: "pie",
                                    },
                                ];

                                Plotly.newPlot("piePlot", data1, layout1);
                            })
                            .catch(function (error) {
                                console.error(error);
                            });
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function checkFeedbackExist() {
        //Fetching Feedback Data
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/feedback/",
        };

        axios
            .request(options)
            .then(function (response) {
                totalFeedback = [];
                totalFeedback.push(response.data);
                console.log(totalFeedback[0].length);
                if (totalFeedback[0].length == 0) {
                    checking = false;
                    if(dark)
                    {
                        no_feedback_style = "color:aliceblue;";
                    }
                    else
                    {
                        no_feedback_style = "color:black;";
                    }
                } else {
                    checking = true;
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        await getData();
        await checkFeedbackExist();
    });
    function togglebtn() {
        if (dark) {
            document.getElementById("main").style.backgroundColor = "aliceblue";
            if (checking == false) {
                no_feedback_style = "color:black;";
            }
            dark = false;
        } else {
            dark = true;
        }
        if (light) {
            document.getElementById("main").style.backgroundColor =
                "rgb(10, 9, 10)";
            if (checking == false) {
                no_feedback_style = "color:aliceblue;";
            }
            light = false;
        } else {
            light = true;
        }
    }
    function setDeleteFeedback(fid) {
        delete_fid = fid;
        toggle1();
    }
    function deleteFeedback(fid) {
        const options = {
            method: "DELETE",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/feedback/fid/" +
                fid,
        };

        axios
            .request(options)
            .then(function (response) {
                toggle2();
            })
            .catch(function (error) {
                console.error(error);
            });
    }

    export let username;
</script>

<main class="main" id="main">
    <nav class="navbar navbar-dark bg-dark navbar-content" style="color: white;">
        <div class="admin">
            <i class="bi bi-person-square"></i>&nbsp Admin
        </div>
        <div class="togglebutton" on:click={togglebtn}>
            {#if dark}
                <p>
                    <i class="bi bi-brightness-high" /><span
                        style="margin-left: 7%;">Light Mode</span
                    >
                </p>
            {/if}
            {#if light}
                <p>
                    <i class="bi bi-moon" /><span style="margin-left: 7%;"
                        >Dark Mode</span
                    >
                </p>
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
    <div class="charts">
        <div id="barPlot" class="bar-plot" />
        <div id="piePlot" class="pie-plot" />
    </div>
    <div class="feedback-container">
        <div class="navbar navbar-dark bg-dark feedback-header">
            <p>All-Feedback</p>
        </div>
        <div class="all-feedbacks">
            {#if checking == false}
                <div class="no-feedback" id="no-feedback" style={no_feedback_style}>
                    <p>No Feedback</p>
                </div>
            {:else}
                {#each totalFeedback[0] as rec, i}
                    <div class="Tooltip">
                        <Tooltip target="delete-{i}" placement="bottom">
                            Delete
                        </Tooltip>
                    </div>
                    <div class="feedback-inner">
                        <div class="card col-sm-6 myinnercard">
                            <table>
                                <h5 class="card-header">
                                    <th style="width: 100%;">{rec.username}</th
                                    ><th
                                        ><i
                                            class="bi bi-trash-fill deletebtn"
                                            id="delete-{i}"
                                            on:click={() =>
                                                setDeleteFeedback(rec.fid)}
                                        /></th
                                    >
                                </h5>
                            </table>
                            <div class="card-body">
                                <h5 class="card-title">
                                    {rec.subject}
                                </h5>
                                <p class="card-text">
                                    {rec.message}
                                </p>
                            </div>
                        </div>
                    </div>
                {/each}
            {/if}
        </div>
    </div>
    <div class="Modals">
        <!-- Confirm Delete -->
        <Modal isOpen={open1}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <p style="font-size: 30px; font-weight: 500;">
                        DELETE FEEDBACK
                    </p>
                </div>
                <div class="delete-symbol-container">
                    <i class="bi bi-exclamation-circle delete-symbol" />
                </div>
                <br />
                <div class="delete-symbol-container">
                    <p class="delete-txt">
                        Are you sure you want to delete this Feedback ?
                    </p>
                </div>
                <Button color="primary" on:click={toggle1}>Cancle</Button>
                <Button
                    color="danger"
                    on:click={toggle1}
                    on:click={() => deleteFeedback(delete_fid)}>Delete</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Deleted Successfully... -->
        <Modal isOpen={open2}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <i class="bi bi-check-circle deleted-symbol" />
                </div>
                <div class="delete-symbol-container">
                    <p class="delete-txt">Feedback is Deleted</p>
                </div>
                <Button
                    color="primary"
                    on:click={toggle2}
                    on:click={checkFeedbackExist}
                    style="width: 100px;">OK</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .admin {
        font-size: 30px;
    }
    .navbar-content {
        display: flex;
        justify-content: space-between;
    }
    .delete-txt {
        font-size: 20px;
    }
    .delete-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .delete-symbol {
        font-size: 70px;
        color: rgb(234, 187, 100);
    }
    .deleted-symbol {
        font-size: 70px;
        color: rgb(39, 138, 63);
    }
    .deletebtn {
        font-size: 30px;
        color: red;
        cursor: pointer;
    }
    .deletebtn:hover {
        color: rgb(190, 3, 3);
    }
    .no-feedback {
        font-size: 30px;
        display: flex;
        justify-content: center;
    }
    .myinnercard {
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .feedback-inner {
        display: flex;
        justify-content: center;
        margin-top: 3%;
    }
    .all-feedbacks {
        margin-top: 3%;
        padding-bottom: 3%;
    }
    .feedback-container {
        margin-top: 5%;
    }
    .feedback-header {
        display: flex;
        justify-content: center;
        color: white;
        font-size: 30px;
        height: 80px;
        line-height: 60px;
    }
    .bar-plot,
    .pie-plot {
        width: 500px;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .charts {
        display: flex;
        justify-content: space-around;
    }
    .togglebtn {
        background-color: rgb(31, 30, 31);
        color: white;
        height: 50px;
        width: 150px;
        border-radius: 15px;
        text-align: center;
        line-height: 50px;
        cursor: pointer;
        margin-left: 90%;
    }
    .togglebutton {
        background-color: rgb(31, 30, 31);
        color: white;
        height: 50px;
        width: 150px;
        border-radius: 15px;
        text-align: center;
        line-height: 50px;
        cursor: pointer;
    }
    .togglebutton:hover {
        background-color: rgb(45, 45, 45);
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
        height: 100%;
    }
    .main-container {
        display: flex;
        justify-content: space-around;
        padding-top: 5%;
        width: 100%;
        height: 100%;
        padding-bottom: 5%;
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
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    @media screen and (max-width: 1100px) {
        .main-container {
            display: block;
            padding-left: 35%;
            padding-bottom: 3%;
        }
        .charts {
            display: block;
            padding-left: 27%;
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
        .pie-plot {
            margin-top: 2%;
        }
    }
    @media screen and (max-width: 950px) {
        .main-container {
            display: block;
            padding-left: 30%;
        }
        .charts {
            display: block;
            padding-left: 21%;
        }
    }
    @media screen and (max-width: 900px) {
        .main-container {
            display: block;
            padding-left: 27%;
        }
        .charts {
            display: block;
            padding-left: 17.5%;
        }
    }
    @media screen and (max-width: 750px) {
        .main-container {
            display: block;
            padding-left: 25%;
        }
        .charts {
            display: block;
            padding-left: 13%;
        }
    }
    @media screen and (max-width: 650px) {
        .main-container {
            display: block;
            padding-left: 20%;
        }
        .charts {
            display: block;
            padding-left: 5%;
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
        .charts {
            display: block;
            padding-left: 8%;
        }
        .pie-plot,
        .bar-plot {
            width: 400px;
            height: 400px;
        }
    }
    @media screen and (max-width: 460px) {
        .main-container {
            display: block;
            padding-left: 10%;
        }
        .charts {
            display: block;
            padding-left: 10%;
        }
        .pie-plot,
        .bar-plot {
            width: 350px;
            height: 350px;
        }
    }
    @media screen and (max-width: 425px) {
        .main-container {
            display: block;
            padding-left: 5%;
        }
        .charts {
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
        .charts {
            display: block;
            padding-left: 3%;
        }
    }
    @media screen and (max-width: 370px) {
        .main-container {
            display: block;
            padding-left: 1.5%;
        }
        .charts {
            display: block;
            padding-left: 1.5%;
        }
    }
</style>
