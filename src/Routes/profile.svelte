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

    function changeName1() {
        btnname1 = "Save";
    }

    function changeName2() {
        btnname2 = "Save";
    }
    let open1 = false;
    let open2 = false;
    let open3 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);

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
        btnname1 = "PLEASE WAIT..."
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
    onMount(async () => {
        userid = localStorage.getItem(username);
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/profileimage/fetchimage/" + userid,
        };
        axios
            .request(options)
            .then(function (response) {
                imageurl = response.data.image;
            })
            .catch(function (error) {
                console.error(error);
            });
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
                <button class="changeImagebtn" on:click={saveImage}>{btnname1}</button
                >
            </div>
        </div>
        <div class="pass-container">
            <div class="pass-header">Edit Account</div>
            <div class="pass-content">
                <button class="changePasswordbtn">{btnname2}</button>
            </div>
        </div>
    </div>
    <div class="Modals">
        <!-- No Image Selected... -->
        <Modal header="Message" isOpen={open1}>
            <ModalBody>Please Select an Image First...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle1}
                    on:click={changeName1}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Profile Image Updated... -->
        <Modal header="Message" isOpen={open2}>
            <ModalBody>Profile Image Updated Successfully...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle2}
                    on:click={changeName1}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Profile Image Set...  -->
        <Modal header="Message" isOpen={open3}>
            <ModalBody>Profile Image Set Successfully...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle3}
                    on:click={changeName1}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
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
        width: 100%;
        text-align: center;
        margin-top: 5%;
    }
    .changePasswordbtn {
        margin-top: 5%;
        border: 2px solid rgb(40, 43, 125);
        border-radius: 20px;
        height: 45px;
        width: 200px;
        font-size: 20px;
        background-color: rgb(77, 80, 148);
        color: white;
    }
    .changePasswordbtn:hover {
        background-color: rgb(40, 43, 125);
        color: white;
    }
    @media screen and (max-width: 1235px) {
        .pass-container {
            height: 500px;
            width: 60%;
        }
        .image-container {
            margin-top: 0.5%;
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
    }
    @media screen and (max-width: 350px) {
        .changePasswordbtn {
            margin-left: 5%;
            width: 60%;
            font-size: 10px;
        }
        .image-container {
            width: 90%;
            margin-left: 5%;
        }
    }
</style>
