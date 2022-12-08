<script>
    let star_rating = "";
    function star1() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s2").style.color = "rgb(200, 205, 200)";
        document.getElementById("s3").style.color = "rgb(200, 205, 200)";
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "one";
    }
    function star2() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s3").style.color = "rgb(200, 205, 200)";
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "two";
    }
    function star3() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "three";
    }
    function star4() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        document.getElementById("s4").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "four";
    }
    function star5() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        document.getElementById("s4").style.color = "rgb(88, 88, 138)";
        document.getElementById("s5").style.color = "rgb(88, 88, 138)";
        star_rating = "five";
    }
</script>

<main>
    <div class="feedback-container">
        <div class="feedback-header">
            <p>Give Your Feedback Here</p>
        </div>
        <div class="star-feedback">
            <i class="bi bi-star-fill staricon" id="s1" on:click={star1} />
            <i class="bi bi-star-fill staricon" id="s2" on:click={star2} />
            <i class="bi bi-star-fill staricon" id="s3" on:click={star3} />
            <i class="bi bi-star-fill staricon" id="s4" on:click={star4} />
            <i class="bi bi-star-fill staricon" id="s5" on:click={star5} />
            <h5>Select Rating For This Application</h5>
        </div>
    </div>
</main>

<style>
    .feedback-container {
        height: 500px;
        width: 45%;
        background-color: white;
        margin-top: -2%;
        margin-left: 3.5%;
        box-shadow: 2px 2px 10px 0px grey;
        border-radius: 1%;
    }
    .feedback-header {
        width: 100%;
        text-align: center;
        font-size: 30px;
        padding-top: 15px;
        border-bottom: 1px solid rgb(215, 215, 239);
    }
    .star-feedback {
        text-align: center;
        font-size: 60px;
        border-bottom: 1px solid rgb(215, 215, 239);
        padding-top: 6%;
        padding-bottom: 8%;
    }
    .staricon {
        cursor: pointer;
        color: rgb(200, 205, 200);
    }
    .staricon:hover {
        color: rgb(88, 88, 138);
    }
    @media screen and (max-width: 1235px) {
        .feedback-container {
            height: 500px;
            width: 60%;
            margin-top: -2%;
        }
    }
    @media screen and (max-width: 995px) {
        .feedback-container {
            margin-left: 20%;
            height: 500px;
        }
    }
    @media screen and (max-width: 825px) {
        .feedback-container {
            margin-left: 15%;
            width: 70%;
        }
    }
    @media screen and (max-width: 770px) {
        .feedback-container {
            margin-left: 10%;
            width: 80%;
            height: 450;
        }
    }
    @media screen and (max-width: 500px) {
        .feedback-container {
            width: 90%;
            margin-left: 5%;
            height: 500px;
            margin-top: -7%;
        }
        .feedback-header {
            font-size: 25px;
        }
        .star-feedback {
            font-size: 40px;
        }
    }
    @media screen and (max-width: 400px) {
        .feedback-container {
            height: 500px;
        }
    }
</style>
