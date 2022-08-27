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
    let current_password = "";
    let new_password = "";
    let confirm_new_password = "";

    let border_current_password =
        "width: 80%; border: 1px solid rgb(130, 130, 224)";
    let border_new_password =
        "width: 80%; border: 1px solid rgb(130, 130, 224)";
    let border_confirm_new_password =
        "width: 80%; border: 1px solid rgb(130, 130, 224)";
    let btnname = "Change-Password";

    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;
    let open5 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);
    const toggle5 = () => (open5 = !open5);

    function changeName() {
        btnname = "Change-Password";
    }

    function validatePassword() {
        btnname = "PLEASE WAIT...";
        if (
            current_password !== "" &&
            new_password !== "" &&
            confirm_new_password !== ""
        ) {
            border_current_password =
                "width: 80%; border: 1px solid rgb(130, 130, 224)";
            border_new_password =
                "width: 80%; border: 1px solid rgb(130, 130, 224)";
            border_confirm_new_password =
                "width: 80%; border: 1px solid rgb(130, 130, 224)";
        }
        if (
            current_password == "" ||
            new_password == "" ||
            confirm_new_password == ""
        ) {
            toggle1();
            if (current_password == "")
                border_current_password = "width: 80%; border: 1px solid red";
            else
                border_current_password =
                    "width: 80%; border: 1px solid rgb(130, 130, 224)";
            if (new_password == "")
                border_new_password = "width: 80%; border: 1px solid red";
            else
                border_new_password =
                    "width: 80%; border: 1px solid rgb(130, 130, 224)";
            if (confirm_new_password == "")
                border_confirm_new_password =
                    "width: 80%; border: 1px solid red";
            else
                border_confirm_new_password =
                    "width: 80%; border: 1px solid rgb(130, 130, 224)";
        } else {
            const options = {
                method: "GET",
                url:
                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/changepassword/username/" +
                    username,
            };

            axios
                .request(options)
                .then(function (response) {
                    if (current_password !== response.data.password) {
                        toggle2();
                    } else {
                        if(new_password.length < 8)
                        {
                            toggle5();
                        }
                        else {
                        if (new_password !== confirm_new_password) {
                            toggle3();
                        } else {
                                let rec = {
                                    username: "",
                                    email: "",
                                    Password: new_password
                                }
                                axios
                                    .put(
                                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/changepassword/updatepassword/" + username,
                                        rec
                                    )
                                    .then(function () {
                                        toggle4();
                                    });
                            }
                        }
                    }
                })
                .catch(function (error) {
                    console.error(error);
                });
        }
    }
    export let username;
</script>

<main>
    <div class="pass-container">
        <div class="pass-header">Change-Password</div>
        <div class="pass-content">
            <FormGroup floating label="Enter Current Password">
                <Input
                    type="password"
                    bind:value={current_password}
                    placeholder="Enter Current Password"
                    style={border_current_password}
                />
            </FormGroup>
            <FormGroup floating label="Enter New Password">
                <Input
                    type="password"
                    bind:value={new_password}
                    placeholder="Enter New Password"
                    style={border_new_password}
                />
            </FormGroup>
            <FormGroup floating label="Confirm New Password">
                <Input
                    type="password"
                    bind:value={confirm_new_password}
                    placeholder="Confirm New Password"
                    style={border_confirm_new_password}
                />
            </FormGroup>
            <button class="changePasswordbtn" on:click={validatePassword}
                >{btnname}</button
            >
        </div>
    </div>
    <div class="Modals">
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open1}>
            <ModalBody>Fields Cannot be empty...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle1}
                    on:click={changeName}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Invalid Current Password -->
        <Modal header="Message" isOpen={open2}>
            <ModalBody>Invalid Current Password...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle2}
                    on:click={changeName}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- new and Confirm password not match -->
        <Modal header="Message" isOpen={open3}>
            <ModalBody>Confirm Password does not match with New Password...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle3}
                    on:click={changeName}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Password Changed Successfully... -->
        <Modal header="Message" isOpen={open4}>
            <ModalBody>Password Changed Successfully...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle4}
                    on:click={changeName}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- length less than 8 -->
        <Modal header="Message" isOpen={open5}>
            <ModalBody>Password must be 8 characters long...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle5}
                    on:click={changeName}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .pass-container {
        height: 420px;
        width: 45%;
        background-color: white;
        margin-top: -2%;
        margin-left: 3.5%;
        box-shadow: 2px 2px 10px 0px grey;
        border-radius: 1%;
    }
    .pass-header {
        text-align: center;
        font-size: 30px;
        border-bottom: 1px solid rgb(215, 215, 239);
        padding: 15px;
    }
    .pass-content {
        margin-left: 15%;
        margin-top: 5%;
    }
    .changePasswordbtn {
        margin-top: 5%;
        margin-left: 18%;
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
            margin-top: 1%;
        }
        .changePasswordbtn {
            margin-left: 22%;
        }
    }
    @media screen and (max-width: 995px) {
        .pass-container {
            margin-left: 20%;
            height: 450px;
        }
        .changePasswordbtn {
            margin-left: 18%;
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
        .changePasswordbtn {
            margin-left: 15%;
        }
    }
    @media screen and (max-width: 500px) {
        .pass-container {
            width: 90%;
            margin-left: 5%;
            height: 450px;
        }
        .changePasswordbtn {
            margin-left: 12%;
        }
    }
    @media screen and (max-width: 450px) {
        .changePasswordbtn {
            margin-left: 8%;
        }
    }
    @media screen and (max-width: 400px) {
        .pass-container {
            height: 430px;
        }
        .changePasswordbtn {
            margin-left: 5%;
        }
    }
    @media screen and (max-width: 350px) {
        .changePasswordbtn {
            margin-left: 5%;
            width: 60%;
            font-size: 10px;
        }
    }
</style>
