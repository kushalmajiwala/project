<script>
    import { Router, Link, Route } from "svelte-routing";
    import { onMount } from "svelte";
    import axios from "axios";
    import { Tooltip } from "sveltestrap";
    import {
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        FormGroup,
        Input,
    } from "sveltestrap";

    let totalCV = [];
    let checking = false;

    //Personal Details Variables
    let fname = "";
    let lname = "";
    let gender = "";
    let dob = "";
    let profession = "";
    let address = "";
    let personal_city = "";
    let personal_state = "";
    let phoneno = "";
    let email = "";
    let personal_pic_url = "";
    let cv_title = "";

    let fname_border = "border: 1px solid #4c89ca;";
    let lname_border = "border: 1px solid #4c89ca;";
    let gender_border = "border: 1px solid #4c89ca;";
    let dob_border = "border: 1px solid #4c89ca;";
    let profession_border = "border: 1px solid #4c89ca;";
    let address_border = "border: 1px solid #4c89ca;";
    let personal_city_border = "border: 1px solid #4c89ca;";
    let personal_state_border = "border: 1px solid #4c89ca;";
    let phoneno_border = "border: 1px solid #4c89ca;";
    let email_border = "border: 1px solid #4c89ca;";
    let cv_title_border = "border: 1px solid #4c89ca;";

    let open1 = false;
    let open2 = false;
    let open3 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);

    export let username;
    export let url = "";

    let userid = localStorage.getItem(username);

    let delete_cvid = "";
    let edit_cvid = "";

    function setDeleteCV(cvid) {
        delete_cvid = cvid;
        toggle1();
    }
    function setEditCV(cvid) {
        edit_cvid = cvid;
        console.log(edit_cvid);
        toggle3();
    }
    // function editCV(cvid) {
    //     console.log(cvid);
    // }
    function deleteCV(cvid) {
        const options = {
            method: "DELETE",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/cvid/" +
                cvid,
        };
        axios
            .request(options)
            .then(function (response) {
                const options = {
                    method: "DELETE",
                    url:
                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/cvid/" +
                        cvid,
                };
                axios
                    .request(options)
                    .then(function (response) {
                        const options = {
                            method: "DELETE",
                            url:
                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/experience/cvid/" +
                                cvid,
                        };
                        axios
                            .request(options)
                            .then(function (response) {
                                const options = {
                                    method: "DELETE",
                                    url:
                                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/education/cvid/" +
                                        cvid,
                                };
                                axios
                                    .request(options)
                                    .then(function (response) {
                                        const options = {
                                            method: "DELETE",
                                            url:
                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/cvid/" +
                                                cvid,
                                        };
                                        axios
                                            .request(options)
                                            .then(function (response) {
                                                toggle2();
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
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function checkCvExist() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                totalCV = [];
                totalCV.push(response.data);
                console.log(totalCV[0].length);
                if (totalCV[0].length == 0) {
                    checking = false;
                } else {
                    checking = true;
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        await checkCvExist();
    });
    function temp() {
        toggle2();
        checkCvExist();
    }
</script>

<main>
    {#if checking == false}
        <div class="none-added">
            <p>No CV Added Yet</p>
        </div>
    {:else}
        <div class="cv-details">
            {#each totalCV[0] as rec}
                <div class="tooltip">
                    <Tooltip target="myedit" placement="bottom">Edit</Tooltip>
                    <Tooltip target="mydownload" placement="bottom">
                        Download
                    </Tooltip>
                    <Tooltip target="myview" placement="bottom">View</Tooltip>
                    <Tooltip target="mydelete" placement="bottom">
                        Delete
                    </Tooltip>
                </div>
                <div class="card inner-cv">
                    <div>
                        <i
                            class="bi bi-file-earmark-person-fill"
                            style="font-size: 70px; color: #598496;"
                        />
                    </div>
                    <div class="inner-content">
                        <div>
                            <p class="cv-title">{rec.cvtitle}</p>
                        </div>
                    </div>
                    <div class="inner-icon">
                        <i
                            class="bi bi-pencil-fill editIcon"
                            id="myedit"
                            on:click={() => setEditCV(rec.cvid)}
                        />
                        <i
                            class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                            id="mydownload"
                        />
                        <i class="bi bi-eye-fill showIcon" id="myview" />
                        <i
                            class="bi bi-trash-fill deleteIcon"
                            id="mydelete"
                            on:click={() => setDeleteCV(rec.cvid)}
                        />
                    </div>
                </div>
            {/each}
            <!-- <Button on:click={temp}>Click</Button> -->
        </div>
    {/if}
    <Router {url}>
        <div class="add-content">
            <Link to="/{username}/manage/addcv" style="text-decoration: none;">
                <i class="bi bi-plus-circle-fill plusbtn" />
            </Link>
            <p class="add-txt">New CV</p>
        </div>
    </Router>
    <div class="Modals">
        <!-- Confirm Delete -->
        <Modal isOpen={open1}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <p style="font-size: 30px; font-weight: 500;">DELETE CV</p>
                </div>
                <div class="delete-symbol-container">
                    <i class="bi bi-exclamation-circle delete-symbol" />
                </div>
                <br />
                <div class="delete-symbol-container">
                    <p class="delete-txt">
                        Are you sure you want to delete this CV ?
                    </p>
                </div>
                <Button color="primary" on:click={toggle1}>Cancle</Button>
                <Button
                    color="danger"
                    on:click={toggle1}
                    on:click={() => deleteCV(delete_cvid)}>Delete</Button
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
                    <p class="delete-txt">Your CV is Deleted...</p>
                </div>
                <Button
                    color="primary"
                    on:click={toggle2}
                    on:click={checkCvExist}
                    style="width: 100px;">OK</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Edit Modal -->

        <Modal isOpen={open3} size="lg">
           
                <div class="personal-page my-content">
                    <ModalHeader>
                        <div class="page-header">
                            <h1>Personal-page</h1>
                        </div>
                    </ModalHeader>
                    <ModalBody>
                    <div class="form-row form-content" style="margin-top: 5%;">
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter First Name">
                                <Input
                                    placeholder="Enter First Name"
                                    style={fname_border}
                                    bind:value={fname}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Last Name">
                                <Input
                                    placeholder="Enter Last Name"
                                    style={lname_border}
                                    bind:value={lname}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div class="form-row form-content">
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Select Gender">
                                <Input
                                    type="select"
                                    name="select"
                                    id="exampleSelect"
                                    style={gender_border}
                                    bind:value={gender}
                                >
                                    <option style="cursor:pointer;">Male</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Female</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Other</option
                                    >
                                </Input>
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Select Date of Birth">
                                <Input
                                    style={dob_border}
                                    type="date"
                                    name="date"
                                    id="exampleDate"
                                    placeholder="date placeholder"
                                    bind:value={dob}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Enter Profession">
                                <Input
                                    placeholder="Enter Profession"
                                    style={profession_border}
                                    bind:value={profession}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div class="form-row form-content">
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Enter Address">
                                <Input
                                    placeholder="Enter Address"
                                    style={address_border}
                                    bind:value={address}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Enter City">
                                <Input
                                    placeholder="Enter City"
                                    style={personal_city_border}
                                    bind:value={personal_city}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-4">
                            <FormGroup floating label="Enter State">
                                <Input
                                    placeholder="Enter State"
                                    style={personal_state_border}
                                    bind:value={personal_state}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div class="form-row form-content">
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Phone Number">
                                <Input
                                    placeholder="Enter Phone Number"
                                    style={phoneno_border}
                                    bind:value={phoneno}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Email">
                                <Input
                                    placeholder="Enter Email"
                                    style={email_border}
                                    bind:value={email}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter CV Title">
                                <Input
                                    placeholder="Enter CV Title"
                                    style={cv_title_border}
                                    bind:value={cv_title}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <!-- <div class="form-row form-content image-form">
                        <div class="image-container" style="margin-bottom: 2%;">
                            <div id="app">
                                <h3 style="margin-bottom: 7%;">
                                    Upload Image For CV
                                </h3>
                                {#if avatar}
                                    <img class="avatar" src={avatar} alt="d" />
                                {:else}
                                    <img
                                        class="avatar"
                                        src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png"
                                        alt=""
                                    />
                                {/if}

                                <label
                                    class="button primary block uploadbtn"
                                    for="single"
                                    ><i class="bi bi-upload" />
                                    {uploading ? "Uploading ..." : "Upload"}
                                </label>
                            </div>
                            <input
                                style="display:none"
                                type="file"
                                id="single"
                                accept="image/*"
                                bind:files
                                bind:this={fileinput}
                                on:change={(e) => onFileSelected(e)}
                                disabled={uploading}
                            />
                        </div>
                    </div>
                    <div class="btncontainer">
                        <p class="next pagebtn" on:click={showEducation}>
                            Next &raquo;
                        </p>
                    </div> -->
                <!-- </div> -->
            </ModalBody>
            <ModalFooter>
                <Button color="primary" on:click={toggle3}>OK</Button>
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
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
    .editIcon,
    .downloadIcon,
    .showIcon,
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
    .deleteIcon {
        color: red;
    }
    .deleteIcon:hover {
        color: red;
        background-color: rgb(5, 58, 67);
    }
    .cv-details {
        position: absolute;
        width: 100%;
        margin-top: -4%;
        padding-left: 19%;
        display: inline-block;
        /* justify-content: center; */
        /* margin-left: 14%; */
    }
    .inner-cv {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
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
    .cv-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
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
        .cv-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
        }
        .inner-cv {
            width: 70%;
        }
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
        .cv-details {
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
        .cv-details {
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
        }
        .cv-details {
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
            margin-top: 14%;
            margin-left: 32%;
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
        .cv-details {
            width: 100%;
            padding-left: 5%;
        }
        .inner-cv {
            width: 95%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
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
            margin-top: 75%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 18%;
        }
        .inner-icon {
            margin-top: -16%;
            margin-left: 15%;
        }
        .cv-details {
            margin-top: -15%;
        }
        .inner-cv {
            margin-top: 5%;
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
