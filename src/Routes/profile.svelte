<script>
    import {
        Badge,
        Form,
        FormGroup,
        Input,
        Label,
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        Tooltip
    } from "sveltestrap";
    import axios from "axios";
    import { createEventDispatcher } from "svelte";
    import { SupabaseStorageClient } from "@supabase/storage-js";
    import { onMount } from "svelte";

    const STORAGE_URL = "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1";
    const SERVICE_KEY =
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR1aXlob21xd2t5c3Fzd2xraXB4Iiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTY1OTYxMjE0MywiZXhwIjoxOTc1MTg4MTQzfQ.AeJz_85Sbinf0ExIyk7V8cgQ8N_eQqOcyyRwf4BkDVU";

    const storageClient = new SupabaseStorageClient(STORAGE_URL, {
        apikey: SERVICE_KEY,
        Authorization: `Bearer ${SERVICE_KEY}`,
    });

    let avatar, fileinput;

    export let path;

    let uploading = false;
    let src;
    let files;
    let image_url = "";
    let imageurl = "";
    let btnname1 = "Save";
    let btnname2 = "Save";
    let userid = "";

    let new_username = "";
    let new_email = "";

    let border_new_username =
        "width: 80%; border: 1px solid rgb(130, 130, 224);";
    let border_new_email = "width: 80%; border: 1px solid rgb(130, 130, 224)";

    let outer_username =
        'width: 80%; border: 1px solid rgb(130, 130, 224);cursor:pointer;';
    let outer_email =
        "width: 80%; border: 1px solid rgb(130, 130, 224);cursor:pointer;margin-top:7%;";

    function changeName1() {
        btnname1 = "Save";
    }

    function changeName2() {
        btnname2 = "Save";
    }
    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;
    let open5 = false;
    let open6 = false;
    let open7 = false;
    let open8 = false;
    let open9 = false;
    let open10 = false;
    let open11 = false;
    let open12 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);
    const toggle5 = () => (open5 = !open5);
    const toggle6 = () => (open6 = !open6);
    const toggle7 = () => (open7 = !open7);
    const toggle8 = () => (open8 = !open8);
    const toggle9 = () => (open9 = !open9);
    const toggle10 = () => (open10 = !open10);
    const toggle11 = () => (open11 = !open11);
    const toggle12 = () => (open12 = !open12);

    const dispatch = createEventDispatcher();

    async function onFileSelected(e) {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = (e) => {
            avatar = e.target.result;
        };
        try {
            uploading = true;

            if (!files || files.length === 0) {
                throw new Error("You must select an image to upload.");
            }

            const file = files[0];
            const fileExt = file.name.split(".").pop();
            const fileName = `${Math.random()}.${fileExt}`;
            const filePath = `${fileName}`;

            const { data, error } = await storageClient
                .from("images")
                .upload(filePath, file);
            if (data) {
                image_url =
                    "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/" +
                    data.Key;
                console.log(image_url);
            }

            if (error) throw error;

            path = filePath;
            dispatch("upload");
        } catch (error) {
            alert(error.message);
        } finally {
            uploading = false;
        }
    }
    function saveImage() {
        btnname1 = "PLEASE WAIT...";
        if (avatar == undefined) {
            toggle1();
        } else {
            userid = localStorage.getItem(username);
            const options = {
                method: "GET",
                url:
                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" +
                    userid,
            };

            axios
                .request(options)
                .then(function (response) {
                    let updateImage = {
                        UserId: userid,
                        image: image_url,
                    };
                    axios
                        .put(
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/" +
                                userid,
                            updateImage
                        )
                        .then(function () {
                            toggle2();
                        });
                })
                .catch(function (error) {
                    let rec = {
                        UserId: userid,
                        image: image_url,
                    };
                    axios
                        .post(
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/saveimage/",
                            rec
                        )
                        .then(function () {
                            toggle3();
                        });
                });
        }
    }
    function refreshPage() {
        let page_url = window.location.href;
        window.location.replace(page_url);
    }
    function editAuthenticationUsername() {
        btnname2 = "PLEASE WAIT...";
        if (new_username !== "") {
            border_new_username =
                "width: 80%; border: 1px solid rgb(130, 130, 224)";
        }
        if (new_username == "") {
            toggle4();
            if (new_username == "")
                border_new_username = "width: 80%; border: 1px solid red";
            else
                border_new_username =
                    "width: 80%; border: 1px solid rgb(130, 130, 224)";
        } else {
            const options = {
                method: "GET",
                url:
                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/editprofile/username/" +
                    new_username,
            };
            axios
                .request(options)
                .then(function (response) {
                    toggle6();
                })
                .catch(function (error) {
                    let rec = {
                        username: new_username,
                        email: "dummyEmail",
                        password: "dummyPassword",
                    };
                    axios
                        .put(
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/editprofile/editusername/" +
                                localStorage.getItem(username),
                            rec
                        )
                        .then(function (response) {
                            toggle9();
                        })
                        .catch(function (error) {
                            toggle8();
                        });
                });
        }
    }
    function editAuthenticationEmail() {
        btnname2 = "PLEASE WAIT...";
        if (new_email !== "") {
            border_new_email =
                "width: 80%; border: 1px solid rgb(130, 130, 224)";
        }
        if (new_email == "") {
            toggle4();
            if (new_email == "")
                border_new_email = "width: 80%; border: 1px solid red";
            else
                border_new_email =
                    "width: 80%; border: 1px solid rgb(130, 130, 224)";
        } else if (!new_email.includes("@") || !new_email.includes(".")) {
            toggle5();
        } else {
            const options = {
                method: "GET",
                url:
                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/editprofile/email/" +
                    new_email,
            };
            axios
                .request(options)
                .then(function (response) {
                    toggle7();
                })
                .catch(function (error) {
                    let rec = {
                        username: "",
                        email: new_email,
                        password: "dummyPassword",
                    };
                    axios
                        .put(
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/editprofile/editemail/" +
                                localStorage.getItem(username),
                            rec
                        )
                        .then(function (response) {
                            toggle10();
                        })
                        .catch(function (error) {
                            toggle8();
                        });
                });
        }
    }
    function loggingOut() {
        if (username !== new_username) {
            localStorage.removeItem(username);
            var page_url = window.location.href;
            let new_url = page_url.replace("/profile", "");
            let final_url = new_url.replace(username, new_username);
            window.location.replace(final_url);
        }
    }
    function startPoint()
    {
        userid = localStorage.getItem(username);
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" +
                userid,
        };
        axios
            .request(options)
            .then(function (response) {
                imageurl = response.data.image;
            })
            .catch(function (error) {
                console.error(error);
            });

        userid = localStorage.getItem(username);
        const options1 = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/editprofile/" +
                userid,
        };
        axios
            .request(options1)
            .then(function (response) {
                new_username = response.data.username;
                new_email = response.data.email;
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        startPoint();
    });
    export let username;
</script>

<main>
    <div class="main-container">
        <div class="image-container">
            <div id="app">
                <h1>Upload Image</h1>
                {#if avatar}
                    <img class="avatar" src={avatar} alt="d" />
                {:else if imageurl !== ""}
                    <img class="avatar" src={imageurl} alt="d" />
                {:else}
                    <img
                        class="avatar"
                        src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png"
                        alt=""
                    />
                {/if}

                <label class="button primary block uploadbtn" for="single"
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
            <div class="save-upload">
                <button class="changeImagebtn" on:click={saveImage}
                    >{btnname1}</button
                >
            </div>
        </div>
        <div class="pass-container">
            <div class="pass-header">Edit Account</div>
            <div>
                <Tooltip target="edittip1" placement="top">Click to Edit</Tooltip>
                <Tooltip target="edittip2" placement="top">Click to Edit</Tooltip>
            </div>
            <div class="pass-content">
                <div on:click={toggle11}>
                    <FormGroup floating label="Update Username" id="edittip1">
                        <Input
                            
                            type="text"
                            bind:value={new_username}
                            placeholder="Enter New Username"
                            style={outer_username}
                            disabled
                        />
                    </FormGroup>
                </div>
                <div on:click={toggle12}>
                    <FormGroup floating label="Update Email" id="edittip2">
                        <Input
                            type="text"
                            bind:value={new_email}
                            placeholder="Enter New Email"
                            style={outer_email}
                            disabled
                        />
                    </FormGroup>
                </div>
            </div>
        </div>
    </div>
    <div class="Modals">
        <!-- No Image Selected... -->
        <Modal isOpen={open1}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                        Please Select an Image First
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle1}
                    on:click={changeName1}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Profile Image Updated... -->
        <Modal isOpen={open2}>
            <ModalFooter>
                <div class="edit-symbol-container">
                    <i class="bi bi-check-all edit-symbol" />
                </div>
                <div class="edit-symbol-container">
                    <p class="edit-txt">Profile Image Updated Successfully</p>
                </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle2}
                    on:click={changeName1}
                    on:click={refreshPage}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Profile Image Set...  -->
        <Modal isOpen={open3}>
            <ModalFooter>
                <div class="edit-symbol-container">
                    <i class="bi bi-check-all edit-symbol" />
                </div>
                <div class="edit-symbol-container">
                    <p class="edit-txt">Profile Image Set Successfully</p>
                </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle3}
                    on:click={changeName1}
                    on:click={refreshPage}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Empty Fields...  -->
        <Modal isOpen={open4}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                      Fields Cannot be empty
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle4}
                    on:click={changeName2}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Invalid Email...  -->
        <Modal isOpen={open5}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                      Invalid Email
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle5}
                    on:click={changeName2}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Username Already exists...  -->
        <Modal isOpen={open6}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                      Username Already Exists
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle6}
                    on:click={changeName2}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Email Already exists...  -->
        <Modal isOpen={open7}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                      Email Already Exists
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle7}
                    on:click={changeName2}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Something Went Wrong...  -->
        <Modal isOpen={open8}>
            <ModalFooter>
                <div class="empty-symbol-container">
                    <i class="bi bi-x-circle empty-symbol" />
                  </div>
                  <div class="empty-symbol-container">
                    <p class="empty-txt">
                        Somthing went wrong...Please Try Again
                    </p>
                  </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle8}
                    on:click={changeName2}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Updated Successfully...  -->
        <Modal isOpen={open9}>
            <ModalFooter>
                <div class="edit-symbol-container">
                    <i class="bi bi-check-all edit-symbol" />
                </div>
                <div class="edit-symbol-container">
                    <p class="edit-txt">Data Updated</p>
                </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle9}
                    on:click={changeName2}
                    on:click={loggingOut}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal isOpen={open10}>
            <ModalFooter>
                <div class="edit-symbol-container">
                    <i class="bi bi-check-all edit-symbol" />
                </div>
                <div class="edit-symbol-container">
                    <p class="edit-txt">Data Updated</p>
                </div>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle10}
                    on:click={changeName2}
                    on:click={refreshPage}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal header="Update Username" isOpen={open11}>
            <ModalBody>
                <FormGroup floating label="Enter New Username">
                    <Input
                        type="text"
                        bind:value={new_username}
                        placeholder="Enter New Username"
                        style={border_new_username}
                    />
                </FormGroup>
            </ModalBody>
            <ModalFooter>
                <Button color="primary" class="float-right" on:click={editAuthenticationUsername}
                    >{btnname2}</Button
                >
                <Button color="danger" class="float-right" on:click={toggle11} on:click={startPoint}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal header="Update Email" isOpen={open12}>
            <ModalBody>
                <FormGroup floating label="Enter New Email">
                    <Input
                        type="text"
                        bind:value={new_email}
                        placeholder="Enter New Email"
                        style={border_new_email}
                    />
                </FormGroup>
            </ModalBody>
            <ModalFooter>
                <Button color="primary" class="float-right" on:click={editAuthenticationEmail}
                    >{btnname2}</Button
                >
                <Button color="danger" class="float-right" on:click={toggle12} on:click={startPoint}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .edit-txt {
        font-size: 20px;
    }
    .edit-symbol-container,
    .empty-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .edit-symbol {
        font-size: 70px;
        color: rgb(39, 138, 63);
    }
    .empty-symbol {
        font-size: 70px;
        color: rgb(237, 59, 59);
    }
    .uploadbtn {
        cursor: pointer;
        color: white;
        background-color: #007bff;
        filter: blur(2%);
        height: 38px;
        width: 120px;
        line-height: 38px;
        text-align: center;
        border-radius: 10px;
        margin-top: 3%;
        border: 1px solid #0772e3;
        filter: saturate(200%);
    }
    .uploadbtn:hover {
        background-color: #0772e3;
    }
    .save-upload {
        width: 100%;
        display: flex;
        justify-content: center;
    }
    .changeImagebtn {
        margin-top: 5%;
        border: 2px solid rgb(40, 125, 125);
        border-radius: 20px;
        height: 45px;
        width: 200px;
        font-size: 20px;
        background-color: rgb(77, 80, 148);
        color: white;
        margin-top: 2%;
    }
    .changeImagebtn:hover {
        background-color: rgb(40, 43, 125);
        color: white;
    }
    #app {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-flow: column;
    }
    .avatar {
        display: flex;
        height: 150px;
        width: 150px;
        margin-top: 2%;
        box-shadow: 0px 0px 5px 0px grey;
    }
    .pass-container {
        height: 420px;
        width: 45%;
        background-color: white;
        margin-top: -2%;
        margin-left: 3.5%;
        box-shadow: 2px 2px 10px 0px grey;
        border-radius: 1%;
    }
    .image-container {
        height: 350px;
        width: 400px;
        background-color: white;
        margin-top: -2%;
        margin-left: 3.5%;
        box-shadow: 2px 2px 10px 0px grey;
        border-radius: 1%;
        margin-bottom: 4%;
    }
    .pass-header {
        text-align: center;
        font-size: 30px;
        border-bottom: 1px solid rgb(215, 215, 239);
        padding: 15px;
    }
    .pass-content {
        margin-left: 15%;
        margin-top: 8%;
    }
    @media screen and (max-width: 1235px) {
        .pass-container {
            height: 500px;
            width: 60%;
        }
        .image-container {
            margin-top: -2%;
        }
    }
    @media screen and (max-width: 995px) {
        .pass-container {
            margin-left: 20%;
            height: 450px;
        }
        .image-container {
            margin-left: 30%;
        }
        .pass-header {
            margin-bottom: 12%;
        }
    }
    @media screen and (max-width: 925px) {
        .image-container {
            margin-left: 28%;
        }
    }
    @media screen and (max-width: 925px) {
        .image-container {
            margin-left: 26%;
        }
    }
    @media screen and (max-width: 825px) {
        .pass-container {
            margin-left: 15%;
            width: 70%;
        }
    }
    @media screen and (max-width: 770px) {
        .pass-container {
            margin-left: 10%;
            width: 80%;
            height: 450;
        }
        .image-container {
            margin-left: 24%;
        }
    }
    @media screen and (max-width: 700px) {
        .image-container {
            margin-left: 22%;
        }
    }
    @media screen and (max-width: 650px) {
        .image-container {
            margin-left: 20%;
        }
    }
    @media screen and (max-width: 630px) {
        .image-container {
            margin-left: 18%;
        }
    }
    @media screen and (max-width: 600px) {
        .image-container {
            margin-left: 16%;
            margin-top: -7%;
        }
        .pass-header {
            margin-bottom: 15%;
        }
    }
    @media screen and (max-width: 580px) {
        .image-container {
            margin-left: 14%;
        }
    }
    @media screen and (max-width: 540px) {
        .image-container {
            margin-left: 22%;
            width: 300px;
        }
    }
    @media screen and (max-width: 500px) {
        .pass-container {
            width: 90%;
            margin-left: 5%;
            height: 450px;
        }
        .image-container {
            margin-left: 18%;
        }
    }
    @media screen and (max-width: 450px) {
        .image-container {
            margin-left: 16%;
        }
        .pass-header {
            margin-bottom: 18%;
        }
    }
    @media screen and (max-width: 420px) {
        .image-container {
            margin-left: 14%;
        }
    }
    @media screen and (max-width: 400px) {
        .pass-container {
            height: 430px;
        }
        .image-container {
            margin-left: 11%;
        }
    }
    @media screen and (max-width: 380px) {
        .image-container {
            margin-left: 5%;
            width: 90%;
        }
        .pass-header {
            margin-bottom: 20%;
        }
    }
    @media screen and (max-width: 350px) {
        .image-container {
            width: 90%;
            margin-left: 5%;
        }
    }
</style>
