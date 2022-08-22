<script>
    import { Router, Link, Route } from "svelte-routing";

    export let username;
    export let url = "";
</script>

<main>
    <div class="none-added">
        <p>No CV Added Yet</p>
    </div>
    <Router {url}>
        <div class="add-content">
            <Link to="/{username}/manage/addcv" style="text-decoration: none;">
            <i
                class="bi bi-plus-circle-fill plusbtn"
            />
            </Link>
            <p class="add-txt">New CV</p>
        </div>
    </Router>
</main>

<style>
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
        color:rgb(100, 86, 167); 
        font-size:90px; 
        cursor: pointer;
    }
    .plusbtn:hover {
        color:rgb(85, 69, 155); 
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
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
    }
    @media screen and (max-width: 1100px) {
        .add-content {
            margin-left: 70%;
            margin-top: 36%;
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
            margin-top: 65%;
        }
        .none-added {
            margin-top: 18%;
            margin-left: 18%;
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

