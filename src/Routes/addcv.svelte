<script>
  import {
    Nav,
    NavItem,
    NavLink,
    TabContent,
    TabPane,
    FormGroup,
    Input,
  } from "sveltestrap";
  import { createEventDispatcher } from "svelte";
  import axios from "axios";
  import { SupabaseStorageClient } from "@supabase/storage-js";
  import { onMount } from "svelte";

  const STORAGE_URL = "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1";
  const SERVICE_KEY =
    "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR1aXlob21xd2t5c3Fzd2xraXB4Iiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTY1OTYxMjE0MywiZXhwIjoxOTc1MTg4MTQzfQ.AeJz_85Sbinf0ExIyk7V8cgQ8N_eQqOcyyRwf4BkDVU";

  const storageClient = new SupabaseStorageClient(STORAGE_URL, {
    apikey: SERVICE_KEY,
    Authorization: `Bearer ${SERVICE_KEY}`,
  });

  let uploading = false;
  let avatar;
  let imageurl = "";
  let files;
  let fileinput;
  let userid = "";
  let image_url = "";

  let personal = true;
  let education = false;
  let skill = false;
  let interest = false;
  let experience = false;

  let personalActive = true;
  let educationActive = false;
  let experienceActive = false;
  let skillActive = false;
  let interestActive = false;

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

  }

  onMount(async () => {
        startPoint();
  });

  function showPersonal() {
    //Showing the page of the content
    personal = true;
    education = false;
    skill = false;
    interest = false;
    experience = false;

    //Showing the Active page tab
    personalActive = true;
    educationActive = false;
    skillActive = false;
    interestActive = false;
    experienceActive = false;
  }
  function showEducation() {
    personal = false;
    education = true;
    skill = false;
    interest = false;
    experience = false;

    personalActive = false;
    educationActive = true;
    skillActive = false;
    interestActive = false;
    experienceActive = false;
  }
  function showExperience() {
    personal = false;
    education = false;
    skill = false;
    interest = false;
    experience = true;

    personalActive = false;
    educationActive = false;
    skillActive = false;
    interestActive = false;
    experienceActive = true;
  }
  function showSkill() {
    personal = false;
    education = false;
    skill = true;
    interest = false;
    experience = false;

    personalActive = false;
    educationActive = false;
    skillActive = true;
    interestActive = false;
    experienceActive = false;
  }
  function showInterest() {
    personal = false;
    education = false;
    skill = false;
    interest = true;
    experience = false;

    personalActive = false;
    educationActive = false;
    skillActive = false;
    interestActive = true;
    experienceActive = false;
  }
  export let username;
  export let path;
</script>

<main>
  <div class="navbar">
    <TabContent vertical pills>
      <div on:click={showPersonal}>
        <TabPane
          tabId="Personal-Details"
          tab="Personal-Details"
          active={personalActive}
        />
      </div>
      <div on:click={showEducation}>
        <TabPane
          tabId="education-Details"
          tab="education-Details"
          active={educationActive}
        />
      </div>
      <div on:click={showExperience}>
        <TabPane
          tabId="experience-Details"
          tab="experience-Details"
          active={experienceActive}
        />
      </div>
      <div on:click={showSkill}>
        <TabPane tabId="skill" tab="skills" active={skillActive} />
      </div>
      <div on:click={showInterest}>
        <TabPane tabId="interest" tab="interest" active={interestActive} />
      </div>
    </TabContent>
  </div>
  <div class="all-content">
    {#if personal}
      <div class="personal-page my-content">
        <div class="page-header">
          <h1>Personal-page</h1>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter First Name">
              <Input placeholder="Enter First Name" />
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Last Name">
              <Input placeholder="Enter Last Name" />
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
                style="cursor:pointer;"
              >
                <option style="cursor:pointer;">Male</option>
                <option style="cursor:pointer;">Female</option>
                <option style="cursor:pointer;">Other</option>
              </Input>
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Select Date of Birth">
              <Input
                style="cursor:pointer;"
                type="date"
                name="date"
                id="exampleDate"
                placeholder="date placeholder"
              />
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter Profession">
              <Input placeholder="Enter Profession" />
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter Address">
              <Input placeholder="Enter Address" />
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter City">
              <Input placeholder="Enter City" />
            </FormGroup>
          </div>
          <div class="form-group col-md-4">
            <FormGroup floating label="Enter State">
              <Input placeholder="Enter State" />
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content">
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Phone Number">
              <Input placeholder="Enter Phone Number" />
            </FormGroup>
          </div>
          <div class="form-group col-md-6">
            <FormGroup floating label="Enter Email">
              <Input placeholder="Enter Email" />
            </FormGroup>
          </div>
        </div>
        <div
          class="form-row form-content"
          style="width: 100%; padding-left: 7%;"
        >
          <div class="form-group" style="width: 80%">
            <FormGroup floating label="Enter CV Title">
              <Input placeholder="Enter CV Title" />
            </FormGroup>
          </div>
        </div>
        <div class="form-row form-content image-form">
          <div class="image-container">
            <div id="app">
              <h3 style="margin-bottom: 7%;">Upload Image For CV</h3>
              {#if avatar}
                <img class="avatar" src={avatar} alt="d" />
              {:else }
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
          </div>
        </div>
        <div class="btncontainer">
          <p class="next pagebtn" on:click={showEducation}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if education}
      <div class="education-page my-content">
        <div class="page-header">
          <h1>Education-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showPersonal}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showExperience}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if experience}
      <div class="education-page my-content">
        <div class="page-header">
          <h1>Experience-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showEducation}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showSkill}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if skill}
      <div class="skill-page my-content">
        <div class="page-header">
          <h1>Skill-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showExperience}>
            &laquo; Previous
          </p>
          <p class="next pagebtn" on:click={showInterest}>Next &raquo;</p>
        </div>
      </div>
    {/if}
    {#if interest}
      <div class="interest-page my-content">
        <div class="page-header">
          <h1>Interest-page</h1>
        </div>
        <div class="pagination">
          <p class="previous pagebtn" on:click={showSkill}>&laquo; Previous</p>
        </div>
      </div>
    {/if}
  </div>
</main>

<style>
  .avatar {
        display: flex;
        height: 150px;
        width: 150px;
        margin-top: 2%;
        margin-left: 25%;
        box-shadow: 0px 0px 2px 0px grey;
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
  .image-container {
        height: 290px;
        width: 300px;
        background-color: white;
        margin-top: 0%;
        margin-left: 31%;
        box-shadow: 0px 0px 7px 0px grey;
        border-radius: 1%;
        margin-bottom: 4%;
        text-align: center;
    }
  .form-content {
    width: 90%;
    margin-left: 5%;
  }
  .btncontainer {
    width: 100%;
    padding-left: 43%;
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
  .pagebtn:hover {
    background-color: rgb(21, 80, 168);
  }
  .pagination {
    padding: 2%;
  }
  .navbar {
    margin-top: -4%;
    padding: 15px;
    font-size: 20px;
    text-align: center;
    background-color: white;
    width: 100%;
    opacity: 0.8;
    font-size: 25px;
    padding-left: 30%;
    box-shadow: 2px 2px 5px 0px grey;
  }
  .page-header {
    width: 100%;
    text-align: center;
    border-bottom: 1px solid rgb(179, 176, 214);
    padding: 10px;
    margin-bottom: 2%;
  }
  .my-content {
    background-color: white;
    height: 950px;
    width: 72%;
    margin-left: 2.2%;
    box-shadow: 2px 2px 5px 0px grey;
  }
  .all-content {
    margin-top: 2%;
    width: 100%;
  }
  @media screen and (min-width: 1350px) {
    .my-content {
      width: 76%;
    }
    .navbar {
      margin-top: -1%;
      padding-left: 33%;
    }
  }
  @media screen and (max-width: 1230px) {
    .navbar {
      padding-left: 41.5%;
      margin-top: 0%;
    }
    .my-content {
      width: 92%;
      margin: 3.5%;
    }
    .image-container {
      margin-left: 35%;
    }
  }
  @media screen and (max-width: 993px) {
    .navbar {
      padding-left: 35%;
      margin-top: -10px;
    }
    .image-container {
      margin-left: 33%;
    }
  }
  @media screen and (max-width: 800px) {
    .navbar {
      margin-top: -1%;
      padding-left: 37%;
    }
    .all-content {
      margin-top: 3%;
    }
    .image-container {
      margin-left: 30%;
    }
  }
  @media screen and (max-width: 770px) {
   .my-content {
    height: 1450px;
   }
   .image-container {
      margin-left: 26%;
    }
    .btncontainer {
      margin-left: -3.5%;
    }
  }
  @media screen and (max-width: 670px) {
    .image-container {
      margin-left: 23%;
    }
    .btncontainer {
      margin-left: -5%;
    }
  }
  @media screen and (max-width: 620px) {
    .navbar {
      padding-left: 30%;
      margin-top: -2%;
    }
    .image-container {
      margin-left: 20%;
    }
  }
  @media screen and (max-width: 570px) {
    .image-container {
      margin-left: 18%;
    }
    .btncontainer {
      margin-left: -6%;
    }
  }
  @media screen and (max-width: 540px) {
    .image-container {
      margin-left: 15%;
    }
    .btncontainer {
      margin-left: -8%;
    }
  }
  @media screen and (max-width: 520px) {
    .navbar {
      padding-left: 28%;
      margin-top: -7%;
    }
    .image-container {
      margin-left: 13%;
    }
    .btncontainer {
      margin-left: -8%;
    }
  }
  @media screen and (max-width: 470px) {
    .navbar {
      padding-left: 26%;
      margin-top: -5%;
    }
    .image-container {
      margin-left: 11.5%;
    }
    .btncontainer {
      margin-left: -8%;
    }
  }
  @media screen and (max-width: 450px) {
    .image-container {
      margin-left: 9.5%;
    }
    .btncontainer {
      margin-left: -10%;
    }
  }
  @media screen and (max-width: 430px) {
    .image-container {
      margin-left: 7%;
    }
    .btncontainer {
      margin-left: -10%;
    }
  }
  @media screen and (max-width: 410px) {
    .navbar {
      padding-left: 24%;
      margin-top: -7%;
    }
    .image-container {
      margin-left: 5%;
    }
    .btncontainer {
      margin-left: -11%;
    }
  }
  @media screen and (max-width: 390px) {
    .navbar {
      padding-left: 22%;
      margin-top: -10%;
    }
    .image-container {
      margin-left: 1%;
    }
    .btncontainer {
      margin-left: -13%;
    }
  }
  @media screen and (max-width: 360px) {
    .navbar {
      padding-left: 19.5%;
      margin-top: -10%;
    }
  }
  @media screen and (max-width: 350px) {
    .navbar {
      padding-left: 20%;
      margin-top: -10%;
    }
  }
  @media screen and (max-width: 330px) {
    .navbar {
      padding-left: 18%;
      margin-top: -10%;
    }
  }
</style>
