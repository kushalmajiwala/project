<script>
    import {
        TabContent,
        TabPane,
        Button,
        Spinner,
        FormGroup,
        Input,
        Label,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        Tooltip,
    } from "sveltestrap";
    import { onMount } from "svelte";
    import axios from "axios";

    let userid;

    let selecttip1;
    let selecttip2;
    let selecttip3;
    let selecttip4;

    //variables for adding format in the database
    let selected_cv = "";
    let selected_letter = "";

    //Modal Variables
    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;

    //Modal Functions
    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);

    //Adding Functionality
    function addcv1() {
        document.getElementById("cv1").style.border =
            "3px solid rgb(50, 252, 50)";
        document.getElementById("cv2").style.border = "none";
        selected_cv = "one";
    }
    function addcv2() {
        document.getElementById("cv2").style.border =
            "3px solid rgb(50, 252, 50)";
        document.getElementById("cv1").style.border = "none";
        selected_cv = "two";
    }
    function addletter1() {
        document.getElementById("letter1").style.border =
            "3px solid rgb(50, 252, 50)";
        document.getElementById("letter2").style.border = "none";
        selected_letter = "one";
    }
    function addletter2() {
        document.getElementById("letter2").style.border =
            "3px solid rgb(50, 252, 50)";
        document.getElementById("letter1").style.border = "none";
        selected_letter = "two";
    }
    function toggleCVSelection() {
        //CV Selection
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/cvformat/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                console.log(response.data.length);
                if (response.data.length == 0) {
                    selected_cv = "one";
                    console.log(selected_cv);
                }
                if (
                    response.data.length > 0 &&
                    response.data[0].format == "one"
                ) {
                    selected_cv = "one";
                    console.log(selected_cv);
                }
                if (
                    response.data.length > 0 &&
                    response.data[0].format == "two"
                ) {
                    selected_cv = "two";
                    console.log(selected_cv);
                }
                if (selected_cv == "one") {
                    document.getElementById("cv1").style.border =
                        "3px solid rgb(50, 252, 50)";
                    document.getElementById("cv2").style.border = "none";
                } else if (selected_cv == "two") {
                    document.getElementById("cv2").style.border =
                        "3px solid rgb(50, 252, 50)";
                    document.getElementById("cv1").style.border =
                        "border: none";
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function toggleLetterSelection() {
        //Letter Selection
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letterformat/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                console.log(response.data.length);
                if (response.data.length == 0) {
                    selected_letter = "one";
                    console.log(selected_letter);
                }
                if (
                    response.data.length > 0 &&
                    response.data[0].format == "one"
                ) {
                    selected_letter = "one";
                    console.log(selected_letter);
                }
                if (
                    response.data.length > 0 &&
                    response.data[0].format == "two"
                ) {
                    selected_letter = "two";
                    console.log(selected_letter);
                }
                if (selected_letter == "one") {
                    document.getElementById("letter1").style.border =
                        "3px solid rgb(50, 252, 50)";
                    document.getElementById("letter2").style.border = "none";
                } else if (selected_letter == "two") {
                    document.getElementById("letter2").style.border =
                        "3px solid rgb(50, 252, 50)";
                    document.getElementById("letter1").style.border = "none";
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function setCV() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/cvformat/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                if (response.data.length > 0) {
                    const options = {
                        method: "PUT",
                        url:
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/cvformat/userid/" +
                            userid,
                        data: {
                            UserId: userid,
                            format: selected_cv,
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
                } else if (response.data.length == 0) {
                    const options = {
                        method: "POST",
                        url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/cvformat",
                        data: {
                            UserId: userid,
                            format: selected_cv
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
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function setLetter() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letterformat/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                if (response.data.length > 0) {
                    const options = {
                        method: "PUT",
                        url:
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letterformat/userid/" +
                            userid,
                        data: {
                            UserId: userid,
                            format: selected_letter,
                        },
                    };
                    axios
                        .request(options)
                        .then(function (response) {
                            toggle4();
                        })
                        .catch(function (error) {
                            console.error(error);
                        });
                } else if (response.data.length == 0) {
                    const options = {
                        method: "POST",
                        url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letterformat",
                        data: {
                            UserId: userid,
                            format: selected_letter
                        },
                    };

                    axios
                        .request(options)
                        .then(function (response) {
                            toggle4();
                        })
                        .catch(function (error) {
                            console.error(error);
                        });
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        userid = localStorage.getItem(username);
    });
    export let username;
</script>

<main>
    <div class="add-container">
        <div
            class="add-content1"
            on:click={toggle1}
            on:click={toggleCVSelection}
        >
            <i class="bi bi-gear-fill seticon" />
            <p class="text-content">Set CV Format</p>
        </div>
        <div
            class="add-content2"
            on:click={toggle2}
            on:click={toggleLetterSelection}
        >
            <i class="bi bi-gear-fill seticon" />
            <p class="text-content">Set Letter Format</p>
        </div>
    </div>
    <div class="Modals">
        <!-- CV Formats -->
        <Modal isOpen={open1} scrollable>
            <ModalHeader style="display:flex; justify-content: center;">
                <p class="format-heading">Select CV Format</p>
            </ModalHeader>
            <ModalBody style="text-align: center;">
                <div>
                    <!-- svelte-ignore a11y-missing-attribute -->
                    <img
                        src="https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/cvformat1.png"
                        on:click={addcv1}
                        id="cv1"
                        bind:this={selecttip1}
                        class="formatpic cv1"
                        height="300"
                        width="200"
                    />
                </div>
                <div class="cv-format">
                    <!-- svelte-ignore a11y-missing-attribute -->
                    <img
                        src="https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/cvformat2.png"
                        on:click={addcv2}
                        id="cv2"
                        bind:this={selecttip2}
                        class="formatpic cv2"
                        height="300"
                        width="200"
                    />
                </div>
            </ModalBody>
            <ModalFooter>
                <Button color="primary" on:click={setCV}>Set CV</Button>
                <Button color="danger" class="float-right" on:click={toggle1}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Letter Formats -->
        <Modal isOpen={open2} scrollable>
            <ModalHeader style="display:flex; justify-content: center;">
                <p class="format-heading">Select Letter Format</p>
            </ModalHeader>
            <ModalBody style="text-align: center;">
                <div>
                    <!-- svelte-ignore a11y-missing-attribute -->
                    <img
                        src="https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/letterformat1.png"
                        on:click={addletter1}
                        id="letter1"
                        bind:this={selecttip3}
                        class="formatpic letter1"
                        height="300"
                        width="200"
                    />
                </div>
                <div class="cv-format">
                    <!-- svelte-ignore a11y-missing-attribute -->
                    <img
                        src="https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/letterformat2.png"
                        on:click={addletter2}
                        id="letter2"
                        bind:this={selecttip4}
                        class="formatpic letter2"
                        height="300"
                        width="200"
                    />
                </div>
            </ModalBody>
            <ModalFooter>
                <Button color="primary" on:click={setLetter}>Set Letter</Button>
                <Button color="danger" class="float-right" on:click={toggle2}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- CV Set -->
        <Modal isOpen={open3}>
            <ModalFooter>
                <div class="set-symbol-container">
                    <i class="bi bi-check-lg set-symbol" />
                </div>
                <div class="set-symbol-container">
                    <p class="set-txt">CV Set Successfully</p>
                </div>
                <Button color="danger" class="float-right" on:click={toggle3} on:click={toggle1}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Letter Set -->
        <Modal isOpen={open4}>
            <ModalFooter>
                <div class="set-symbol-container">
                    <i class="bi bi-check-lg set-symbol" />
                </div>
                <div class="set-symbol-container">
                    <p class="set-txt">Letter Set Successfully</p>
                </div>
                <Button color="danger" class="float-right" on:click={toggle4} on:click={toggle2}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
    <div class="Tooltips" style="display: none;">
        <Tooltip target={selecttip1} placement="right">Select</Tooltip>
        <Tooltip target={selecttip2} placement="right">Select</Tooltip>
        <Tooltip target={selecttip3} placement="right">Select</Tooltip>
        <Tooltip target={selecttip4} placement="right">Select</Tooltip>
    </div>
</main>

<style>
    .set-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .set-symbol {
        font-size: 70px;
        color: rgb(18, 143, 18);
    }
    .seticon {
        color: rgb(100, 86, 167);
        font-size: 60px;
        margin-left: 7%;
    }
    .formatpic {
        box-shadow: 0px 0px 5px 0px grey;
        cursor: pointer;
    }
    .cv-format {
        margin-top: 5%;
    }
    .format-heading {
        font-size: 25px;
    }
    .add-container {
        display: flex;
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
        box-shadow: 3px 3px 10px 1px grey;
    }
    .add-content2:hover {
        opacity: 1;
        box-shadow: 3px 3px 10px 1px grey;
    }
    .add-content1:hover .text-content {
        color: rgb(6, 6, 89);
    }
    .add-content2:hover .text-content {
        color: rgb(6, 6, 89);
    }
    .add-content1:hover .seticon {
        color: rgb(32, 32, 139);
    }
    .add-content2:hover .seticon {
        color: rgb(32, 32, 139);
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
