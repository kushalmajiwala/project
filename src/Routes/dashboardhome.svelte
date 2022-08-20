<main>
    <div class="add-container">
        <div class="add-content">
            <i class="bi bi-plus-circle-fill" style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"/>
            <span class="text-content">Create New CV</span>
        </div>
        <div class="add-content">
            <i class="bi bi-plus-circle-fill" style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"/>
            <span class="text-content">Create New Cover Letter</span>
        </div>
    </div>
</main>

<style>
    .add-container {
        display: flex;
        justify-content: left;
    }
    .add-content {
        height: 140px;
        width: 320px;
        background-color: white;
        margin-left: 8.5%;
        margin-top: 5%;
        line-height: 140px;
        cursor: pointer;
        box-shadow: 3px 3px 5px 0px grey;
        border-radius: 3%;
    }
    .text-content {
        font-size: 20px;
        position: absolute;
        margin-left: 1%;
    }
    @media screen and (max-width: 1250px) {
        .add-content {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 12%;
            margin-top: 10%;
        }
    }
    @media screen and (max-width: 993px) {
        .add-content {
            margin-left: 10%;
        }
    }
    @media screen and (max-width: 940px) {
        .add-content {
            margin-left: 7%;
        }
    }
    @media screen and (max-width: 865px) {
        .add-content {
            margin-left: 5%;
        }
    }
    @media screen and (max-width: 800px) {
        .add-content {
            margin-left: 3%;
        }
    }
    @media screen and (max-width: 750px) {
        .add-container {
            display: inline-block;
            justify-content: center;
            margin-top: -11%;
        }
        .add-content {
            margin-left: 57%;
        }
    }
    @media screen and (max-width: 670px) {
        .add-content {
            margin-left: 45%;
        }
    }
    @media screen and (max-width: 630px) {
        .add-content {
            margin-left: 35%;
        }
    }
    @media screen and (max-width: 570px) {
        .add-content {
            margin-left: 27%;
        }
    }
    @media screen and (max-width: 500px) {
        .add-content {
            margin-left: 20%;
        }
    }
    @media screen and (max-width: 470px) {
        .add-content {
            margin-left: 15%;
        }
    }
    @media screen and (max-width: 440px) {
        .add-content {
            margin-left: 10%;
        }
    }
    @media screen and (max-width: 410px) {
        .add-content {
            margin-left: 5%;
        }
    }
    @media screen and (max-width: 380px) {
        .add-content {
            margin-left: 3.5%;
        }
    }
    @media screen and (max-width: 370px) {
        .add-container {
            width: 100%;
        }
        .add-content {
            margin-left: 5%;
            width: 90%;
        }
    }
</style>
