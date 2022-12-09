<script>
    import axios from "axios";
    import { Chart } from 'chart.js/auto'
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

    //Round Graph Logic
    var xValues = ["Italy", "France", "Spain", "USA", "Argentina"];
    var yValues = [55, 49, 44, 24, 15];
    var barColors = ["#b91d47", "#00aba9", "#2b5797", "#e8c3b9", "#1e7145"];

    new Chart("myChart", {
        type: "pie",
        data: {
            labels: xValues,
            datasets: [
                {
                    backgroundColor: barColors,
                    data: yValues,
                },
            ],
        },
        options: {
             
        },
    });

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

    export let username;
</script>

<main class="main">
    <div class="main-container">
        <div class="total-user" />
        <div class="total-cv" />
        <div class="total-letter" />
    </div>
    <div class="round-graph">
        <canvas id="myChart" style="width:100%;max-width:600px" />
    </div>
</main>

<style>
    .main {
        background-color: rgb(253, 250, 250);
    }
    .main-container {
        display: flex;
        justify-content: space-around;
        padding-top: 5%;
        width: 100%;
        height: 100%;
        padding-bottom: 20%;
        background-color: green;
    }
    .total-user {
        background-color: rgb(214, 162, 131);
    }
    .total-cv {
        background-color: rgb(106, 145, 188);
    }
    .total-letter {
        background-color: rgb(122, 189, 121);
    }
    .total-user,
    .total-cv,
    .total-letter {
        height: 200px;
        width: 350px;
        border-radius: 25px;
    }
   
</style>
