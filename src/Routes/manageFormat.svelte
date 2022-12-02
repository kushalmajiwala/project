<script>
</script>

<main>
    <div class="add-container">
        <div class="add-content1">
            <i
                class="bi bi-gear-fill"
                style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"
            />
            <p class="text-content">Set CV Format</p>
        </div>
        <div class="add-content2">
            <i
                class="bi bi-gear-fill"
                style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"
            />
            <p class="text-content">Set Letter Format</p>
        </div>
    </div>
</main>

<style>
    .add-container {
        display: flex;
        /* justify-content: center; */
        /* margin-left: 10px; */
        width: 100%;
        margin-top: -40px;
        padding-left: 2%;
    }
    .add-content1,
    .add-content2 {
        display: block;
        justify-content: center;
        align-items: center;
        width: 300px;
        height: 150px;
        background-color: white;
        margin-left: 20px;
        margin-top: 15px;
        text-align: center;
        box-shadow: 3px 3px 5px 0px grey;
        border-radius: 3%;
        opacity: 0.9;
        cursor: pointer;
        padding-top: 1%;
        padding-right: 2%;
    }
    .add-content1:hover {
        opacity: 1;
    }
    .add-content1:hover {
        opacity: 1;
    }
    .text-content {
        font-size: 20px;
        margin-left: 8%;
    }
    @media screen and (max-width: 820px) {
       .add-container {
        display: block;
        padding-left: 30%;
       }
    }
    @media screen and (max-width: 670px) {
       .add-container {
        padding-left: 25%;
       }
    }
    @media screen and (max-width: 600px) {
       .add-container {
        padding-left: 20%;
       }
    }
    @media screen and (max-width: 500px) {
       .add-container {
        padding-left: 15%;
       }
    }
    @media screen and (max-width: 450px) {
       .add-container {
        padding-left: 10%;
       }
    }
    @media screen and (max-width: 400px) {
       .add-container {
        padding-left: 5%;
       }
    }
    @media screen and (max-width: 360px) {
       .add-container {
        padding-left: 2%;
       }
    }
</style>
