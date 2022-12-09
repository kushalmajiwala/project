<script>
    // @ts-nocheck

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

    let userid;

    //Modal Variables
    let open1 = false;
    let open2 = false;
    let open3 = false;

    //Modal Functions
    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);

    let star_rating = "";
    let subject = "";
    let message = "";

    let subject_border = "border: 1px solid #4c89ca;";
    let message_border = "border: 1px solid #4c89ca; height: 200px;";

    function star1() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s2").style.color = "rgb(200, 205, 200)";
        document.getElementById("s3").style.color = "rgb(200, 205, 200)";
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "1";
    }
    function star2() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s3").style.color = "rgb(200, 205, 200)";
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "2";
    }
    function star3() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "3";
    }
    function star4() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        document.getElementById("s4").style.color = "rgb(88, 88, 138)";
        // Removing Blue
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        star_rating = "4";
    }
    function star5() {
        // Making Blue
        document.getElementById("s1").style.color = "rgb(88, 88, 138)";
        document.getElementById("s2").style.color = "rgb(88, 88, 138)";
        document.getElementById("s3").style.color = "rgb(88, 88, 138)";
        document.getElementById("s4").style.color = "rgb(88, 88, 138)";
        document.getElementById("s5").style.color = "rgb(88, 88, 138)";
        star_rating = "5";
    }
    function resetAll() {
        document.getElementById("s1").style.color = "rgb(200, 205, 200)";
        document.getElementById("s2").style.color = "rgb(200, 205, 200)";
        document.getElementById("s3").style.color = "rgb(200, 205, 200)";
        document.getElementById("s4").style.color = "rgb(200, 205, 200)";
        document.getElementById("s5").style.color = "rgb(200, 205, 200)";
        subject = "";
        message = "";
        subject_border = "border: 1px solid #4c89ca;";
        message_border = "border: 1px solid #4c89ca; height: 200px;";
    }
    function feedbackValidate() {
        if (subject !== "" && message !== "") {
            subject_border = "border: 1px solid #4c89ca;";
            message_border = "border: 1px solid #4c89ca; height: 200px;";
        }
        if (subject == "" || message == "") {
            if (subject == "") {
                subject_border = "border: 1px solid red;";
            } else {
                subject_border = "border: 1px solid #4c89ca;";
            }
            if (message == "") {
                message_border = "border: 1px solid red; height: 200px;";
            } else {
                message_border = "border: 1px solid #4c89ca; height: 200px;";
            }
            toggle1();
            return false;
        }
        return true;
    }
    function ratingValidate() {
        if (star_rating == "") {
            toggle2();
            return false;
        }
        return true;
    }
    function submitFeedback() {
        if (ratingValidate()) {
            if (feedbackValidate()) {
                const options = {
                    method: "POST",
                    url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/feedback/",
                    data: {
                        UserId: userid,
                        rating: star_rating,
                        username: username,
                        subject: subject,
                        message: message
                    },
                };

                axios
                    .request(options)
                    .then(function (response) {
                        toggle3();
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            }
        }
    }
    onMount(async () => {
        userid = localStorage.getItem(username);
    });
    export let username;
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
        <div class="feedback-symbol">
            <i class="bi bi-chat-left-text-fill" />
            <h5
                style="margin-top: -10px; color: black; margin-left: 2%; margin-right: 2%;"
            >
                If You find any issue in the application, feel free to contact
                us
            </h5>
        </div>
        <div class="feedback-content">
            <div style="width: 80%;">
                <FormGroup floating label="Add Subject">
                    <Input placeholder="Add Subject" bind:value={subject} style={subject_border} />
                </FormGroup>
                <FormGroup>
                    <div
                        style="width: 100%; display:flex; justify-content: center;"
                    >
                        <Label for="exampleText" style="font-size: 25px;"
                            >Add-Message</Label
                        >
                    </div>
                    <Input
                        type="textarea"
                        name="text"
                        id="exampleText"
                        maxlength="200"
                        bind:value={message}
                        style={message_border}
                    />
                </FormGroup>
            </div>
        </div>
        <div class="feedback-submit">
            <Button
                color="primary"
                on:click={submitFeedback}
                style="height: 45px; width: 127px; font-size: 18px;"
                >Send-Message</Button
            >
        </div>
    </div>
    <div class="Modals">
        <Modal isOpen={open1}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                </div>
                <div class="empty-symbol-container">
                    <p class="empty-txt">
                        Fields Cannot be empty on Feedback Page
                    </p>
                </div>
                <Button color="danger" class="float-right" on:click={toggle1}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal isOpen={open2}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                </div>
                <div class="empty-symbol-container">
                    <p class="empty-txt">Please Give the ratings</p>
                </div>
                <Button color="danger" class="float-right" on:click={toggle2}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal isOpen={open3}>
            <ModalFooter>
                <div class="create-symbol-container">
                    <i class="bi bi-check-lg create-symbol" />
                  </div>
                  <div class="create-symbol-container">
                    <p class="create-txt">
                        Thanks For giving Feedback
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle3}
                    on:click={resetAll}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .empty-symbol-container, .create-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .empty-symbol {
        font-size: 70px;
        color: rgb(237, 59, 59);
    }
    .create-symbol {
        font-size: 70px;
        color: rgb(18, 143, 18);
    }
    .feedback-submit {
        padding-top: 5%;
        text-align: center;
    }
    .feedback-content {
        width: 100%;
        padding-top: 5%;
        padding-left: 16%;
        padding-bottom: 5%;
        border-bottom: 1px solid rgb(215, 215, 239);
    }
    .feedback-symbol {
        text-align: center;
        font-size: 100px;
        color: rgb(88, 88, 138);
        padding-top: 2%;
        padding-bottom: 2%;
        border-bottom: 1px solid rgb(215, 215, 239);
    }
    .feedback-container {
        height: 1000px;
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
            width: 60%;
            margin-top: -2%;
        }
    }
    @media screen and (max-width: 995px) {
        .feedback-container {
            margin-left: 20%;
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
        }
    }
    @media screen and (max-width: 500px) {
        .feedback-container {
            width: 90%;
            margin-left: 5%;
            margin-top: -7%;
        }
        .feedback-header {
            font-size: 25px;
        }
        .star-feedback {
            font-size: 40px;
        }
    }
    /* @media screen and (max-width: 400px) {
        .feedback-container {
            height: 500px;
        }
    } */
</style>
