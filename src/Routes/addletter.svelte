<script>
    // @ts-nocheck
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
    } from "sveltestrap";

    function progressStop() {
        progress = false;
    }

    //Modal Variables
    let open1 = false;
    let open2 = false;
    let open3 = false;

    //Modal Functions
    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);

    //Personal Page Variables
    let personal_information = true;
    let letter_title = "";
    let fname = "";
    let lname = "";
    let personal_address = "";
    let phone = "";
    let email = "";
    let profession = "";

    //Personal Page Border Variables
    let letter_title_border = "border: 1px solid #4c89ca;";
    let fname_border = "border: 1px solid #4c89ca;";
    let lname_border = "border: 1px solid #4c89ca;";
    let personal_address_border = "border: 1px solid #4c89ca;";
    let phone_border = "border: 1px solid #4c89ca;";
    let email_border = "border: 1px solid #4c89ca;";
    let profession_border = "border: 1px solid #4c89ca;";

    //Recipient Page Variables
    let recipient_information = false;
    let recipient_name = "";
    let recipient_gender = "";
    let company_name = "";
    let company_address = "";
    let company_city = "";
    let company_state = "";

    //Recipient page Border Variables
    let recipient_name_border = "border: 1px solid #4c89ca;";
    let recipient_gender_border = "border: 1px solid #4c89ca;";
    let company_name_border = "border: 1px solid #4c89ca;";
    let company_address_border = "border: 1px solid #4c89ca;";
    let company_city_border = "border: 1px solid #4c89ca;";
    let company_state_border = "border: 1px solid #4c89ca;";

    //Letter Page variables
    let letter_page = false;
    let letter_content = "";

    //Letter Page Border Variables
    let letter_content_border = "border: 1px solid #4c89ca; height: 400px;";

    let progress = false;

    //Showing Functions
    function showPersonal() {
        personal_information = true;
        recipient_information = false;
        letter_page = false;
    }
    function showRecipient() {
        personal_information = false;
        recipient_information = true;
        letter_page = false;
    }
    function showLettercontent() {
        personal_information = false;
        recipient_information = false;
        letter_page = true;
    }

    //Validation Functions
    function personalValidate() {
        if (
            letter_title !== "" &&
            fname !== "" &&
            lname !== "" &&
            personal_address !== "" &&
            phone !== "" &&
            email !== "" &&
            profession !== ""
        ) {
            letter_title_border = "border: 1px solid #4c89ca;";
            fname_border = "border: 1px solid #4c89ca;";
            lname_border = "border: 1px solid #4c89ca;";
            personal_address_border = "border: 1px solid #4c89ca;";
            phone_border = "border: 1px solid #4c89ca;";
            email_border = "border: 1px solid #4c89ca;";
            profession_border = "border: 1px solid #4c89ca;";
        }
        if (
            letter_title == "" ||
            fname == "" ||
            lname == "" ||
            personal_address == "" ||
            phone == "" ||
            email == "" ||
            profession == ""
        ) {
            if (letter_title == "")
                letter_title_border = "border: 1px solid red;";
            else letter_title_border = "border: 1px solid #4c89ca;";
            if (fname == "") fname_border = "border: 1px solid red;";
            else fname_border = "border: 1px solid #4c89ca;";
            if (lname == "") lname_border = "border: 1px solid red;";
            else lname_border = "border: 1px solid #4c89ca;";
            if (personal_address == "")
                personal_address_border = "border: 1px solid red;";
            else personal_address_border = "border: 1px solid #4c89ca;";
            if (phone == "") phone_border = "border: 1px solid red;";
            else phone_border = "border: 1px solid #4c89ca;";
            if (email == "") email_border = "border: 1px solid red;";
            else email_border = "border: 1px solid #4c89ca;";
            if (profession == "") profession_border = "border: 1px solid red;";
            else profession_border = "border: 1px solid #4c89ca;";
            toggle1();
            showPersonal();
            return false;
        } else {
            return true;
        }
    }
    function recipientValidate() {
        if (
            recipient_name !== "" &&
            recipient_gender !== "" &&
            company_name !== "" &&
            company_address !== "" &&
            company_city !== "" &&
            company_state !== ""
        ) {
            recipient_name_border = "border: 1px solid #4c89ca;";
            recipient_gender_border = "border: 1px solid #4c89ca;";
            company_name_border = "border: 1px solid #4c89ca;";
            company_address_border = "border: 1px solid #4c89ca;";
            company_city_border = "border: 1px solid #4c89ca;";
            company_state_border = "border: 1px solid #4c89ca;";
        }
        if(recipient_name == "" ||
            recipient_gender == "" ||
            company_name == "" ||
            company_address == "" ||
            company_city == "" ||
            company_state == "")
        {
            if(recipient_name == "") recipient_name_border = "border: 1px solid red;";
            else recipient_name_border = "border: 1px solid #4c89ca;";
            if(recipient_gender == "") recipient_gender_border = "border: 1px solid red;";
            else recipient_gender_border = "border: 1px solid #4c89ca;";
            if(company_name == "") company_name_border = "border: 1px solid red;";
            else company_name_border = "border: 1px solid #4c89ca;";
            if(company_address == "") company_address_border = "border: 1px solid red;";
            else company_address_border = "border: 1px solid #4c89ca;";
            if(company_city == "") company_city_border = "border: 1px solid red;";
            else company_city_border = "border: 1px solid #4c89ca;";
            if(company_state == "") company_state_border = "border: 1px solid red;";
            else company_state_border = "border: 1px solid #4c89ca;";
            toggle2();
            showRecipient();
            return false;
        }
        else
        {
            return true;
        }
    }
    function lettercontentValidate() {
        if(letter_content !== "")
        {
            letter_content_border = "border: 1px solid #4c89ca;height: 400px;";   
        }
        if(letter_content == "")
        {
            letter_content_border = "border: 1px solid red;height: 400px;";
            toggle3();
            showLettercontent();
            return false;
        }
        else
        {
            return true;
        }
    }

    function resetAll()
    {
        letter_title = "";
        fname = "";
        lname = "";
        personal_address = "";
        phone = "";
        email = "";
        profession = "";

        recipient_name = "";
        recipient_gender = "";
        company_name = "";
        company_address = "";
        company_city = "";
        company_state = "";

        letter_content = "";
    }

    //submit Letter
    function submitLetter() {
        progress = true;
        if(personalValidate())
        {
            if(recipientValidate())
            {
                if(lettercontentValidate())
                {
            
                }
            }
        }
    }
</script>

<main>
    <div class="navbar">
        <TabContent vertical pills>
            <div on:click={showPersonal}>
                <TabPane
                    tabId="Personal-Information"
                    tab="Personal-Information"
                    active
                />
            </div>
            <div on:click={showRecipient}>
                <TabPane
                    tabId="Recipient-Information"
                    tab="Recipient-Information"
                />
            </div>
            <div on:click={showLettercontent}>
                <TabPane tabId="Letter-Content" tab="Letter-Content" />
            </div>
        </TabContent>
    </div>
    <!-- Personal-Information-Page -->
    {#if personal_information}
        <div class="my-content">
            <div class="page-header">
                <p>Personal-Information</p>
            </div>
            <div
                class="form-row form-content"
                style="width: 100%; padding-left: 10%;"
            >
                <div class="form-group" style="width: 90%">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Letter Title"
                    >
                        <Input
                            placeholder="Enter Letter Title"
                            style={letter_title_border}
                            bind:value={letter_title}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter First Name"
                    >
                        <Input
                            placeholder="Enter First Name"
                            style={fname_border}
                            bind:value={fname}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Last Name"
                    >
                        <Input
                            placeholder="Enter Last Name"
                            style={lname_border}
                            bind:value={lname}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Address"
                    >
                        <Input
                            placeholder="Enter Address"
                            style={personal_address_border}
                            bind:value={personal_address}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Phone Number"
                    >
                        <Input
                            placeholder="Enter Phone Number"
                            style={phone_border}
                            bind:value={phone}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Email"
                    >
                        <Input
                            placeholder="Enter Email"
                            style={email_border}
                            bind:value={email}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Profession"
                    >
                        <Input
                            placeholder="Enter Profession"
                            style={profession_border}
                            bind:value={profession}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="btncontainer">
                <p class="next pagebtn" on:click={showRecipient}>
                    Next &raquo;
                </p>
            </div>
        </div>
    {/if}
    <!-- Recipient-Information-Page -->
    {#if recipient_information}
        <div class="my-content">
            <div class="page-header">
                <p>Recipient-Information</p>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Recipient Name"
                    >
                        <Input
                            placeholder="Enter Recipient Name"
                            style={recipient_name_border}
                            bind:value={recipient_name}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Select Recipient Gender"
                    >
                        <Input
                            type="select"
                            name="select"
                            id="exampleSelect"
                            style={recipient_gender_border}
                            bind:value={recipient_gender}
                        >
                            <option style="cursor:pointer;">Male</option>
                            <option style="cursor:pointer;">Female</option>
                        </Input>
                    </FormGroup>
                </div>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Company Name"
                    >
                        <Input
                            placeholder="Enter Company Name"
                            style={company_name_border}
                            bind:value={company_name}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Company Address"
                    >
                        <Input
                            placeholder="Enter Company Address"
                            style={company_address_border}
                            bind:value={company_address}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="form-row form-content" style="padding-left: 9.5%;">
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Company City"
                    >
                        <Input
                            placeholder="Enter Company City"
                            style={company_city_border}
                            bind:value={company_city}
                        />
                    </FormGroup>
                </div>
                <div class="form-group col-md-6">
                    <FormGroup
                        style="text-align:center"
                        floating
                        label="Enter Company State"
                    >
                        <Input
                            placeholder="Enter Company State"
                            style={company_state_border}
                            bind:value={company_state}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="btncontainer">
                <p class="previous pagebtn" on:click={showPersonal}>
                    &laquo; Previous
                </p>
                <p class="next pagebtn" on:click={showLettercontent}>
                    Next &raquo;
                </p>
            </div>
        </div>
    {/if}
    <!-- Letter-Content -->
    {#if letter_page}
        <div class="my-content">
            <div class="page-header">
                <p>Letter-Content</p>
            </div>
            <div style="width: 100%; margin-top: 80px; padding-left: 16%;">
                <div style="width: 80%;">
                    <FormGroup>
                        <div
                            style="width: 100%; display:flex; justify-content: center;"
                        >
                            <Label for="exampleText" style="font-size: 25px;"
                                >Add-Content</Label
                            >
                        </div>
                        <Input
                            type="textarea"
                            name="text"
                            id="exampleText"
                            maxlength="800"
                            bind:value={letter_content}
                            style={letter_content_border}
                        />
                    </FormGroup>
                </div>
            </div>
            <div class="btncontainer">
                <p class="previous pagebtn" on:click={showRecipient}>
                    &laquo; Previous
                </p>
                <Button
                    on:click={submitLetter}
                    color="success"
                    style="width: 120px; height: 50px; border-radius: 15px; font-size: 22px; margin-top: 25px;"
                >
                    {#if progress}
                        <p><Spinner size="sm" /> Saving</p>
                    {:else}
                        <p><i class="bi bi-file-earmark-check-fill" /> Save</p>
                    {/if}
                </Button>
            </div>
        </div>
    {/if}
    <div class="Modals">
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open1}>
            <ModalBody
                >Fields Cannot be empty on Personal-Information Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle1}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
         <!-- empty-modal -->
         <Modal header="Message" isOpen={open2}>
            <ModalBody
                >Fields Cannot be empty on Recipient-Information Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle2}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open3}>
            <ModalBody
                >Field Cannot be empty on Letter-Content Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle3}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .form-content {
        margin-top: 0%;
        width: 91.5%;
    }
    .btncontainer {
        width: 100%;
        display: flex;
        justify-content: space-around;
    }
    .pagebtn {
        padding: 10px;
        background-color: rgb(70, 127, 212);
        cursor: pointer;
        border-radius: 15px;
        width: 120px;
        text-align: center;
        font-size: 20px;
        color: white;
        margin-top: 25px;
    }
    .navbar {
        margin-top: -3.5%;
        padding: 15px;
        text-align: center;
        background-color: white;
        width: 100%;
        opacity: 0.8;
        font-size: 20px;
        padding-left: 32%;
        box-shadow: 2px 2px 5px 0px grey;
    }
    .my-content {
        background-color: white;
        height: 830px;
        width: 74%;
        margin-left: 2%;
        box-shadow: 2px 2px 5px 0px grey;
        margin-top: 2%;
    }
    .page-header {
        width: 100%;
        display: flex;
        justify-content: center;
        white-space: nowrap;
        font-size: 30px;
        border-bottom: 1px solid rgb(179, 176, 214);
        padding-top: 15px;
        font-family: sans-serif;
        margin-bottom: 6%;
    }
    @media screen and (min-width: 1350px) {
        .navbar {
            margin-top: -2.5%;
            padding-left: 33%;
        }
        .my-content {
            width: 76%;
            margin-left: 3%;
        }
    }
    @media screen and (max-width: 1230px) {
        .navbar {
            padding-left: 41.5%;
            margin-top: -3%;
        }
        .my-content {
            width: 90%;
            margin-left: 4.8%;
        }
    }
    @media screen and (max-width: 993px) {
        .navbar {
            padding-left: 35%;
            margin-top: -5%;
        }
        .form-content {
            margin-left: 0.5%;
        }
    }
    @media screen and (max-width: 800px) {
        .navbar {
            margin-top: -5%;
            padding-left: 37%;
        }
    }
    @media screen and (max-width: 620px) {
        .navbar {
            padding-left: 30%;
            margin-top: -5%;
        }
        .my-content {
            margin-top: 3%;
        }
        .page-header {
            margin-bottom: 10%;
        }
    }
    @media screen and (max-width: 520px) {
        .navbar {
            padding-left: 28%;
            margin-top: -7%;
        }
    }
    @media screen and (max-width: 470px) {
        .navbar {
            padding-left: 26%;
            margin-top: -9%;
        }
        .my-content {
            margin-top: 4%;
        }
    }
    @media screen and (max-width: 410px) {
        .navbar {
            padding-left: 24%;
            margin-top: -7%;
        }
    }
    @media screen and (max-width: 390px) {
        .navbar {
            padding-left: 22%;
            margin-top: -12%;
        }
        .my-content {
            margin-top: 5%;
        }
    }
    @media screen and (max-width: 360px) {
        .navbar {
            padding-left: 19.5%;
            margin-top: -13%;
        }
    }
    @media screen and (max-width: 350px) {
        .navbar {
            padding-left: 20%;
            margin-top: -14%;
        }
    }
    @media screen and (max-width: 330px) {
        .navbar {
            padding-left: 18%;
            margin-top: -15%;
        }
    }
</style>
