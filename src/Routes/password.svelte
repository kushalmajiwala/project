<script>
    import { Badge, Form, FormGroup, Input, Label } from 'sveltestrap';
</script>
<main>
    <div class="pass-container">
        <div class="pass-header">
            Change Password
        </div>
        <div class="pass-content">
            <FormGroup floating label="Enter Current Password">
                <Input type="password" placeholder="Enter Current Password" style="width: 80%; border: 1px solid rgb(130, 130, 224)" />
            </FormGroup>
            <FormGroup floating label="Enter New Password">
                <Input type="password" placeholder="Enter New Password" style="width: 80%;  border: 1px solid rgb(130, 130, 224)"/>
            </FormGroup>
            <FormGroup floating label="Confirm New Password">
                <Input type="password" placeholder="Confirm New Password" style="width: 80%;  border: 1px solid rgb(130, 130, 224)"/>
            </FormGroup>
            <button class="changePasswordbtn">Change-Password</button>
        </div>
    </div>
</main>
<style>
    .pass-container {
        height: 400px;
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