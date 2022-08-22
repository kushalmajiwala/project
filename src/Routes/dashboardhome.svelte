<script>
    import { Router, Link, Route } from "svelte-routing";

    export let username;
    export let url = "";
</script>

<main>
    <Router {url}>
        <div class="add-container">
            <Link to="/{username}/manage/addcv" style="text-decoration: none;">
                <div class="add-content1">
                    <i
                        class="bi bi-plus-circle-fill"
                        style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"
                    />
                    <span class="text-content">Create New CV</span>
                </div>
            </Link>
            <Link to="/{username}/manage/addletter" style="text-decoration: none;">
            <div class="add-content2">
                <i
                    class="bi bi-plus-circle-fill"
                    style="color:rgb(100, 86, 167); font-size:60px; margin-left: 7%;"
                />
                <span class="text-content">Create New Cover Letter</span>
            </div>
            </Link>
        </div>
    </Router>
</main>

<style>
    .add-container {
        display: flex;
        justify-content: left;
    }
    .add-content1 {
        height: 140px;
        width: 320px;
        background-color: white;
        margin-left: 40%;
        margin-top: 20%;
        line-height: 140px;
        cursor: pointer;
        box-shadow: 3px 3px 5px 0px grey;
        border-radius: 3%;
        opacity: 0.9;
    }
    .add-content2 {
        height: 140px;
        width: 320px;
        background-color: white;
        margin-left: 55%;
        margin-top: 20%;
        line-height: 140px;
        cursor: pointer;
        box-shadow: 3px 3px 5px 0px grey;
        border-radius: 3%;
        opacity: 0.9;
    }
    .add-content1:hover {
        opacity: 1;
    }
    .add-content1:hover {
        opacity: 1;
    }
    .text-content {
        font-size: 20px;
        position: absolute;
        margin-left: 1%;
    }
    @media screen and (min-width: 1350px) {
        .add-content1 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 70%;
            margin-top: 52%;
        }
        .add-content2 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 90%;
            margin-top: 52%;
        }
    }
    @media screen and (max-width: 1250px) {
        .add-content1 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 60%;
            margin-top: 40%;
        }
        .add-content2 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 80%;
            margin-top: 40%;
        }
    }
    @media screen and (max-width: 1140px) {
        .add-content1 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 55%;
            margin-top: 40%;
        }
        .add-content2 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 75%;
            margin-top: 40%;
        }
    }
    @media screen and (max-width: 1110px) {
        .add-content1 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 50%;
            margin-top: 40%;
        }
        .add-content2 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 70%;
            margin-top: 40%;
        }
    }
    @media screen and (max-width: 1050px) {
        .add-content1 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 40%;
            margin-top: 40%;
        }
        .add-content2 {
            height: 160px;
            width: 350px;
            line-height: 160px;
            margin-left: 60%;
            margin-top: 40%;
        }
    }
    @media screen and (max-width: 993px) {
        .add-content1 {
            margin-left: 35%;
            margin-top: 17%;
        }
        .add-content2 {
            margin-left: 45%;
            margin-top: 17%;
        }
    }
    @media screen and (max-width: 940px) {
        .add-content1 {
            margin-left: 20%;
        }
        .add-content2 {
            margin-left: 40%;
        }
    }
    @media screen and (max-width: 865px) {
        .add-content1 {
            margin-left: 12%;
        }
        .add-content2 {
            margin-left: 23%;
        }
    }
    @media screen and (max-width: 800px) {
        .add-content1 {
            margin-left: 8%;
        }
        .add-content2 {
            margin-left: 17%;
        }
    }
    @media screen and (max-width: 770px) {
        .add-content1 {
            margin-left: 5%;
        }
        .add-content2 {
            margin-left: 14%;
        }
    }
    @media screen and (max-width: 750px) {
        .add-container {
            display: inline-block;
            justify-content: center;
            margin-top: -11%;
        }
        .add-content1 {
            margin-left: 57%;
        }
        .add-content2 {
            margin-left: 57%;
        }
    }
    @media screen and (max-width: 670px) {
        .add-content1 {
            margin-left: 45%;
        }
        .add-content2 {
            margin-left: 45%;
        }
    }
    @media screen and (max-width: 630px) {
        .add-content1 {
            margin-left: 35%;
        }
        .add-content2 {
            margin-left: 35%;
        }
    }
    @media screen and (max-width: 570px) {
        .add-content1 {
            margin-left: 27%;
        }
        .add-content2 {
            margin-left: 27%;
        }
    }
    @media screen and (max-width: 500px) {
        .add-content1 {
            margin-left: 20%;
        }
        .add-content2 {
            margin-left: 20%;
        }
    }
    @media screen and (max-width: 470px) {
        .add-content1 {
            margin-left: 15%;
        }
        .add-content2 {
            margin-left: 15%;
        }
    }
    @media screen and (max-width: 440px) {
        .add-content1 {
            margin-left: 10%;
            margin-top: 10%;
        }
        .add-content2 {
            margin-left: 10%;
            margin-top: 10%;
        }
    }
    @media screen and (max-width: 410px) {
        .add-content1 {
            margin-left: 5%;
        }
        .add-content2 {
            margin-left: 5%;
        }
    }
    @media screen and (max-width: 380px) {
        .add-content1 {
            margin-left: 3.5%;
        }
        .add-content2 {
            margin-left: 3.5%;
        }
    }
    @media screen and (max-width: 370px) {
        .add-container {
            width: 100%;
        }
        .add-content1 {
            margin-left: 5%;
            width: 90%;
        }
        .add-content2 {
            margin-left: 5%;
            width: 90%;
        }
    }
</style>
